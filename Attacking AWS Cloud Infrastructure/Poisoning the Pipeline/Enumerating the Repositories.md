# Port Forwarding with Socat

Explore as Authenticated User
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Enumerating-the-Repositories-1.png)

Jenkinsfile in static_content
>``` shell
>01  pipeline {
>02      agent any   
>03      // TODO automate the building of this later
>04      stages {
>05          stage('Build') {
>06              steps {
>07                  echo 'Building..'
>08              }
>09          }
>10          stage('Test') {
>11              steps {
>12                  echo 'Testing..'
>13              }
>14          }
>15          stage('Deploy') {
>16              steps {
>17                  echo 'Deploying....'
>18              }
>19          }
>20      }
>21  }      
>```

Reviewing the image-transform Repository
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Enumerating-the-Repositories-2.png)

Jenkinsfile in image-transform
>``` shell
>01 pipeline {
>02    agent any
>03
>04    stages {
>05
>06      
>07      stage('Validate Cloudfront File') {
>08        steps {
>09          withAWS(region:'us-east-1', credentials:'aws_key') {
>10              cfnValidate(file:'image-processor-template.yml')
>11          }
>12        }
>13      }
>14
>15      stage('Create Stack') {
>16        steps {
>17          withAWS(region:'us-east-1', credentials:'aws_key') {
>18              cfnUpdate(
>19                  stack:'image-processor-stack', 
>20                  file:'image-processor-template.yml', 
>21                  params:[
>22                      'OriginalImagesBucketName=original-images-lgudbhv8syu2tgbk',
>23                      'ThumbnailImageBucketName=thumbnail-images--lgudbhv8syu2tgbk'
>24                  ], 
>25                  timeoutInMinutes:10, 
>26                  pollInterval:1000)
>27          }
>28        }
>29      }
>30    }
>31  } 
>```

S3 buckets in Cloudformation
>``` shell
>01  AWSTemplateFormatVersion: '2010-09-09'
>02
>03  Parameters:
>04    OriginalImagesBucketName:
>05      Type: String
>06      Description: Enter the name for the Original Images Bucket
>07    ThumbnailImageBucketName:
>08      Type: String
>09      Description: Enter the name for the Thumbnail Images Bucket
>10
>11  Resources:
>12    # S3 buckets for storing original and thumbnail images
>13    OriginalImagesBucket:
>14      Type: AWS::S3::Bucket
>15      Properties:
>16        BucketName: !Ref OriginalImagesBucketName
>17        AccessControl: Private
>18    ThumbnailImagesBucket:
>19      Type: AWS::S3::Bucket
>20      Properties:
>21        BucketName: !Ref ThumbnailImageBucketName
>22        AccessControl: Private
>```

Lambda Function in Cloudformation
>``` shell
>24    ImageProcessorFunction:
>25      Type: 'AWS::Lambda::Function'
>26      Properties:
>27        FunctionName: ImageTransform
>28        Handler: index.lambda_handler
>29        Runtime: python3.9
>30        Role: !GetAtt ImageProcessorRole.Arn
>31        MemorySize: 1024
>32        Environment:
>33          Variables:
>34            # S3 bucket names
>35            ORIGINAL_IMAGES_BUCKET: !Ref OriginalImagesBucket
>36            THUMBNAIL_IMAGES_BUCKET: !Ref ThumbnailImagesBucket
>37        Code:
>38          ZipFile: |
>39            import boto3
>40            import os
>41            import json
>42
>43            SOURCE_BUCKET = os.environ['ORIGINAL_IMAGES_BUCKET']
>44            DESTINATION_BUCKET = os.environ['THUMBNAIL_IMAGES_BUCKET']
>45
>46
>47            def lambda_handler(event, context):
>48                s3 = boto3.resource('s3')
>49
>50                # Loop through all objects in the source bucket
>51                for obj in s3.Bucket(SOURCE_BUCKET).objects.all():
>52                    # Get the file key and create a new Key object
>53                    key = obj.key
>54                    copy_source = {'Bucket': SOURCE_BUCKET, 'Key': key}
>55                    new_key = key
>56                    
>57                    # Copy the file from the source bucket to the destination bucket
>58                    # TODO: this should process the image and shrink it to a more desirable size
>59                    s3.meta.client.copy(copy_source, DESTINATION_BUCKET, new_key)
>60                return {
>61                    'statusCode': 200,
>62                    'body': json.dumps('Success')
>63                }
>65    ImageProcessorScheduleRule:
>66      Type: AWS::Events::Rule
>67      Properties:
>68        Description: "Runs the ImageProcessorFunction daily"
>69        ScheduleExpression: rate(1 day)
>70        State: ENABLED
>71        Targets:
>72          - Arn: !GetAtt ImageProcessorFunction.Arn
>73            Id: ImageProcessorFunctionTarget
>```

IAM policy for Lambda function in Cloudformation
>``` shell
>74    ImageProcessorRole:
> 75      Type: AWS::IAM::Role
> 76      Properties:
> 77        AssumeRolePolicyDocument:
> 78          Version: '2012-10-17'
> 79          Statement:
> 80          - Effect: Allow
> 81            Principal:
> 82              Service:
> 83              - lambda.amazonaws.com
> 84            Action:
> 85            - sts:AssumeRole
> 86        Path: "/"
> 87        Policies:
> 88        - PolicyName: ImageProcessorLogPolicy
> 89          PolicyDocument:
> 90            Version: '2012-10-17'
> 91            Statement:
> 92            - Effect: Allow
> 93              Action:
> 94              - logs:CreateLogGroup
> 95              - logs:CreateLogStream
> 96              - logs:PutLogEvents
> 97              Resource: "*"
> 98        - PolicyName: ImageProcessorS3Policy
> 99          PolicyDocument:
>100            Version: '2012-10-17'
>101            Statement:
>102            - Effect: Allow
>103              Action:
>104                - "s3:PutObject"
>105                - "s3:GetObject"
>106                - "s3:AbortMultipartUpload"
>107                - "s3:ListBucket"
>108                - "s3:DeleteObject"
>109                - "s3:GetObjectVersion"
>110                - "s3:ListMultipartUploadParts"
>111              Resource:
>112                - !Sub arn:aws:s3:::${OriginalImagesBucket}
>113                - !Sub arn:aws:s3:::${OriginalImagesBucket}/*
>114                - !Sub arn:aws:s3:::${ThumbnailImagesBucket}
>115                - !Sub arn:aws:s3:::${ThumbnailImagesBucket}/*
>```

Reviewing Webhooks Under Settings
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Enumerating-the-Repositories-3.png)

Review Webhook Triggers
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Enumerating-the-Repositories-4.png)

Lab 1 - Discover the flag in the repository.
>``` shell
>
>```
>

Lab 2 - Many SCMs, including Gitea, support multiple types of Webhooks, including Slack, Gogs, Telegram, Discord, etc. What type of webhook is configured on the image-transform repository?
>Gogs

