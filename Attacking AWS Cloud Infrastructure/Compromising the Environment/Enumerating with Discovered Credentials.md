# Enumerating with Discovered Credentials

Configuring stolen-s3 AWS S3 Profile
>``` shell
>kali@kali:~$ aws configure --profile=stolen-s3
>
># ========== Expected Result ==========
>AWS Access Key ID [None]: AKIAUBHUBEGIMWGUDSWQ
>AWS Secret Access Key [None]: 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
>Default region name [None]: us-east-1
>Default output format [None]: 
># =====================================
>```

Getting the Account ID and Username
>``` shell
>kali@kali:~$ aws --profile=stolen-s3 sts get-caller-identity
>
># ========== Expected Result ==========
>{
>    "UserId": "AIDAUBHUBEGIFYDAVQPLB",
>    "Account": "347537569308",
>    "Arn": "arn:aws:iam::277537169808:user/s3_explorer"
>}
># =====================================
>```

Failing to List the User Policies Attached to Configured User
>``` shell
>kali@kali:~$ aws --profile=stolen-s3 iam list-user-policies --user-name s3_explorer
>
># ========== Expected Result ==========
>An error occurred (AccessDenied) when calling the ListUserPolicies operation: User: arn:aws:iam::277537169808:user/s3_explorer is not authorized to perform: iam:ListUserPolicies on resource: user s3_explorer because no identity-based policy allows the iam:ListUserPolicies action
># =====================================
>```

Listing S3 Bucket for Company Directory
>``` shell
>kali@kali:~$ aws --profile=stolen-s3 s3 ls company-directory-9b58rezp3vvkf90f
>
># ========== Expected Result ==========
>2023-07-06 13:49:19        117 Alen.I.vcf
>2023-07-06 13:49:19        118 Goran.B.vcf
>2023-07-06 13:49:19        117 Zeljko.B.vcf
># =====================================
>```

Listing all Buckets From stolen-s3 Account
>``` shell
>kali@kali:~$ aws --profile=stolen-s3 s3api list-buckets
>
># ========== Expected Result ==========
>{
>    "Buckets": [
>        {
>            "Name": "company-directory-9b58rezp3vvkf90f",
>            "CreationDate": "2023-07-06T16:21:16+00:00"
>        },
>        {
>            "Name": "tf-state-9b58rezp3vvkf90f",
>            "CreationDate": "2023-07-06T16:21:16+00:00"
>        }
>    ]
>    ...
>}
># =====================================
>```
