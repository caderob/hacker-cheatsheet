# Extracting Insights from Enumeration Data

Getting the "admin-alice" IAM user details
>``` shell
>kali@kali:~$ aws --profile target iam get-account-authorization-details --filter User Group --query "UserDetailList[?UserName=='admin-alice']"
>
># ========== Expected Result ==========
>[
>    {
>        "Path": "/admin/",
>        "UserName": "admin-alice",
>        "UserId": "AIDAQOMAIGYU3FWX3JOFP",
>        "Arn": "arn:aws:iam::123456789012:user/admin/admin-alice",
>        "GroupList": [
>            "amethyst_admin",
>            "admin"
>        ],
>        "AttachedManagedPolicies": [],
>        "Tags": [
>            {
>                "Key": "Project",
>                "Value": "amethyst"
>            }
>        ]
>    }
>]
># =====================================
>```

Getting the "admin" and "amethyst_admin" User Groups details
>``` shell
>kali@kali:~$ aws --profile target iam get-account-authorization-details --filter User Group --query "GroupDetailList[?GroupName=='admin']"
>
># ========== Expected Result ==========
>[
>    {
>        "Path": "/admin/",
>        "GroupName": "admin",
>        "GroupId": "AGPAQOMAIGYUXBR7QGLLN",
>        "Arn": "arn:aws:iam::123456789012:group/admin/admin",
>        "GroupPolicyList": [],
>        "AttachedManagedPolicies": [
>            {
>                "PolicyName": "AdministratorAccess",
>                "PolicyArn": "arn:aws:iam::aws:policy/AdministratorAccess"
>            }
>        ]
>    }
>]
># =====================================
>
>kali@kali:~$ aws --profile target iam get-account-authorization-details --filter User Group --query "GroupDetailList[?GroupName=='amethyst_admin']"
>
># ========== Expected Result ==========
>[
>    {
>        "Path": "/amethyst/",
>        "GroupName": "amethyst_admin",
>        "GroupId": "AGPAQOMAIGYUX23CDL3AN",
>        "Arn": "arn:aws:iam::123456789012:group/amethyst/amethyst_admin",
>        "GroupPolicyList": [],
>        "AttachedManagedPolicies": [
>            {
>                "PolicyName": "amethyst_admin",
>                "PolicyArn": "arn:aws:iam::123456789012:policy/amethyst/amethyst_admin"
>            }
>        ]
>    }
>]
># =====================================
>```

Analyzing the AdminitratorAccess Policy Document
>``` shell
>{
>  "Version" : "2012-10-17",
>  "Statement" : [
>    {
>      "Effect" : "Allow",
>      "Action" : "*",
>      "Resource" : "*"
>    }
>  ]
>}
>```

Getting the "amethyst_admin" policy statements
>``` shell
>kali@kali:~$ aws --profile target iam get-account-authorization-details --filter LocalManagedPolicy --query "Policies[?PolicyName=='amethyst_admin']"
>
># ========== Expected Result ==========
>[
>    {
>        "PolicyName": "amethyst_admin",
>        "PolicyId": "ANPAQOMAIGYUUA3PZUK57",
>        "Arn": "arn:aws:iam::123456789012:policy/amethyst/amethyst_admin",
>        "Path": "/amethyst/",
>        "DefaultVersionId": "v7",
>        "AttachmentCount": 1,
>        "PermissionsBoundaryUsageCount": 0,
>        "IsAttachable": true,
>        "PolicyVersionList": [
>            {
>                "Document": {
>                    "Statement": [
>                        {
>                            "Action": "iam:*",
>                            "Effect": "Allow",
>                            "Resource": [
>                                "arn:aws:iam::123456789012:user/amethyst/*",
>                                "arn:aws:iam::123456789012:group/amethyst/*",
>                                "arn:aws:iam::123456789012:role/amethyst/*",
>                                "arn:aws:iam::123456789012:policy/amethyst/*"
>                            ],
>                            "Sid": "AllowAllIAMActionsInUserPath"
>                        },
>                        {
>                            "Action": "iam:*",
>                            "Condition": {
>                                "StringEquals": {
>                                    "aws:ResourceTag/Project": "amethyst"
>                                }
>                            },
>                            "Effect": "Allow",
>                            "Resource": "arn:aws:iam::*:user/*",
>                            "Sid": "AllowAllIAMActionsInGroupMembers"
>                        },
>                        {
>                            "Action": [
>                                "ec2:*",
>                                "lambda:*"
>                            ],
>                            "Condition": {
>                                "StringEquals": {
>                                    "aws:ResourceTag/Project": "amethyst"
>                                }
>                            },
>                            "Effect": "Allow",
>                            "Resource": "*",
>                            "Sid": "AllowAllActionsInTaggedResources"
>                        },
>                        {
>                            "Action": [
>                                "ec2:*",
>                                "lambda:*"
>                            ],
>                            "Condition": {
>                                "StringEquals": {
>                                    "aws:RequestTag/Project": "amethyst"
>                                }
>                            },
>                            "Effect": "Allow",
>                            "Resource": "*",
>                            "Sid": "AllowAllActionsInTaggedResources2"
>                        },
>                        {
>                            "Action": "s3:*",
>                            "Effect": "Allow",
>                            "Resource": [
>                                "arn:aws:s3:::amethyst*",
>                                "arn:aws:s3:::amethyst*/*"
>                            ],
>                            "Sid": "AllowAllS3ActionsInPath"
>                        }
>                    ],
>                    "Version": "2012-10-17"
>                },
>                "IsDefaultVersion": true,
>            },
>    }
>]
># =====================================
>```

Drawing the path to privilege escalation from the admin-cbarton IAM user
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Extracting-Insights-from-Enumeration-Data-1.png)

Lab 1 - In the context of the provided text, which of the following is an indicator that the IAM user admin-alice might be a fully-privileged user?
>C) The user is a member of the 'admin' group

Lab 2 - Which strategy uses attributes like tags to determine permissions in public cloud environments?
>B) Attribute-Based Access Control (ABAC)

Lab 3 - Run an analysis, like we did in the example for this section, to find a user in another group that has dangerous permissions that could lead to privilege escalation in the environment. Write the username as the answer.
>
