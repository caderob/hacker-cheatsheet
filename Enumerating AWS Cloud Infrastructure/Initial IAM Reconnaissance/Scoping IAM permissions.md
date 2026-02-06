# Scoping IAM permissions

Running the get-caller-identity command
>``` shell
>kali@kali:~$ aws --profile target sts get-caller-identity
>
># ========== Expected Result ==========
>{
>    "UserId": "AIDAQOMAIGYUYNMOIF46I",
>    "Account": "123456789012",
>    "Arn": "arn:aws:iam::123456789012:user/support/clouddesk-plove"
>}
># =====================================
>```

Listing inline and managed policies associated with an IAM user
>``` shell
>kali@kali:~$ aws --profile target iam list-user-policies --user-name clouddesk-plove
>
># ========== Expected Result ==========
>{
>    "PolicyNames": []
>}
># =====================================
>
>kali@kali:~$ aws --profile target iam list-attached-user-policies --user-name clouddesk-plove
>
># ========== Expected Result ==========
>{
>    {
>    "AttachedPolicies": [
>        {
>            "PolicyName": "deny_challenges_access",
>            "PolicyArn": "arn:aws:iam::123456789012:policy/deny_challenges_access"
>        }
>    ]
>}
>}
># =====================================
>```

Listing the groups to which the user belongs
>``` shell
>kali@kali:~$ aws --profile target iam list-groups-for-user --user-name clouddesk-plove
>
># ========== Expected Result ==========
>{
>    "Groups": [
>        {
>            "Path": "/support/",
>            "GroupName": "support",
>            "GroupId": "AGPAQOMAIGYUSHSVDSYIP",
>            "Arn": "arn:aws:iam::123456789012:group/support/support",
>        }
>    ]
>}
># =====================================
>```

Listing inline and managed policies associated with an IAM group
>``` shell
>kali@kali:~$ aws --profile target iam list-group-policies --group-name support
>
># ========== Expected Result ==========
>{
>    "PolicyNames": []
>}
># =====================================
>
>kali@kali:~$ aws --profile target iam list-attached-group-policies --group-name support
>
># ========== Expected Result ==========
>{
>    "AttachedPolicies": [
>        {
>            "PolicyName": "SupportUser",
>            "PolicyArn": "arn:aws:iam::aws:policy/job-function/SupportUser"
>        }
>    ]
>}
># =====================================
>```

Listing a policy version
>``` shell
>kali@kali:~$ aws --profile target iam list-policy-versions --policy-arn "arn:aws:iam::aws:policy/job-function/SupportUser"
>
># ========== Expected Result ==========
>{
>    "Versions": [
>        {
>            "VersionId": "v8",
>            "IsDefaultVersion": true
>        },
>        {
>            "VersionId": "v7",
>            "IsDefaultVersion": false,
>        },
>...
># =====================================
>```

Listing a policy definition by its version
>``` shell
>kali@kali:~$ aws --profile target iam get-policy-version --policy-arn arn:aws:iam::aws:policy/job-function/SupportUser --version-id v8
>
># ========== Expected Result ==========
>{
>    "PolicyVersion": {
>        "Document": {
>            "Version": "2012-10-17",
>            "Statement": [
>                {
>                    "Action": [
>                        "support:*",
>                        "acm:DescribeCertificate",
>                        "acm:GetCertificate",
>                        "acm:List*",
>                        "acm-pca:DescribeCertificateAuthority",
>                        "autoscaling:Describe*",
>...
>                        "workdocs:Describe*",
>                        "workmail:Describe*",
>                        "workmail:Get*",
>                        "workspaces:Describe*"
>                    ],
>                    "Effect": "Allow",
>                    "Resource": "*"
>                }
>            ]
>        },
>        "VersionId": "v8",
>        "IsDefaultVersion": true,
>...
>    }
>}
># =====================================
>```

Lab 1 - What command is used to list the inline policies associated with an IAM user in AWS?
>B) list-user-policies

Lab 2 - What does the wildcard "*" represent in an IAM policy's action statement?
>C) It allows all actions that match the specified prefix

Lab 3 - Use the challenge Profile in AWS CLI to scope the level of actions allowed to run in the EC2 service. Run the permitted actions to list or describe Resources. You will find a Tag Key named proof in one of the resources you can list. Enter the value of the Tag Key.
>``` shell
>
>```
>
