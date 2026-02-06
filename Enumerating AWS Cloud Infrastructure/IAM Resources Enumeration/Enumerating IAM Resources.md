# Enumerating IAM Resources

Getting the permissions related to IAM service of the SupportUser policy
>``` shell
>kali@kali:~$ aws --profile target iam get-policy-version --policy-arn arn:aws:iam::aws:policy/job-function/SupportUser --version-id v8 | grep "iam"
>
># ========== Expected Result ==========
>                        "iam:GenerateCredentialReport",
>                        "iam:GenerateServiceLastAccessedDetails",
>                        "iam:Get*",
>                        "iam:List*",
># =====================================
>```

Getting available IAM subcommands
>``` shell
>kali@kali:~$ aws --profile target iam help | grep -E "list-|get-|generate-"
>
># ========== Expected Result ==========
>       o generate-credential-report
>       o generate-organizations-access-report
>       o generate-service-last-accessed-details
>       o get-access-key-last-used
>       o get-account-authorization-details
>       o get-account-password-policy
>       o get-account-summary
>       o get-context-keys-for-custom-policy
>       o get-context-keys-for-principal-policy
>       o get-credential-report
>       o get-group
>       o get-group-policy
>       o get-instance-profile
>       o get-login-profile
>       o get-open-id-connect-provider
>       o get-organizations-access-report
>       o get-policy
>       o get-policy-version
>       o get-role
>       o get-role-policy
>       o get-saml-provider
>       o get-server-certificate
>       o get-service-last-accessed-details
>       o get-service-last-accessed-details-with-entities
>       o get-service-linked-role-deletion-status
>       o get-ssh-public-key
>       o get-user
>       o get-user-policy
>       o list-access-keys
>       o list-account-aliases
>       o list-attached-group-policies
>       o list-attached-role-policies
>       o list-attached-user-policies
>       o list-entities-for-policy
>       o list-group-policies
>       o list-groups
>       o list-groups-for-user
>       o list-instance-profile-tags
>       o list-instance-profiles
>       o list-instance-profiles-for-role
>       o list-mfa-device-tags
>       o list-mfa-devices
>       o list-open-id-connect-provider-tags
>       o list-open-id-connect-providers
>       o list-policies
>       o list-policies-granting-service-access
>       o list-policy-tags
>       o list-policy-versions
>       o list-role-policies
>       o list-role-tags
>       o list-roles
>       o list-saml-provider-tags
>       o list-saml-providers
>       o list-server-certificate-tags
>       o list-server-certificates
>       o list-service-specific-credentials
>       o list-signing-certificates
>       o list-ssh-public-keys
>       o list-user-policies
>       o list-user-tags
>       o list-users
>       o list-virtual-mfa-devices
># =====================================
>```

Getting the IAM Account Summary
>``` shell
>kali@kali:~$ aws --profile target iam get-account-summary | tee account-summary.json
>
># ========== Expected Result ==========
>aws --profile target iam get-account-summary
>{
>    "SummaryMap": {
>        "GroupPolicySizeQuota": 5120,
>        "InstanceProfilesQuota": 1000,
>        "Policies": 8,
>        "GroupsPerUserQuota": 10,
>        "InstanceProfiles": 0,
>        "AttachedPoliciesPerUserQuota": 10,
>        "Users": 18,
>        "PoliciesQuota": 1500,
>        "Providers": 1,
>        "AccountMFAEnabled": 0,
>        "AccessKeysPerUserQuota": 2,
>        "AssumeRolePolicySizeQuota": 2048,
>        "PolicyVersionsInUseQuota": 10000,
>        "GlobalEndpointTokenVersion": 1,
>        "VersionsPerPolicyQuota": 5,
>        "AttachedPoliciesPerGroupQuota": 10,
>        "PolicySizeQuota": 6144,
>        "Groups": 8,
>        "AccountSigningCertificatesPresent": 0,
>        "UsersQuota": 5000,
>        "ServerCertificatesQuota": 20,
>        "MFADevices": 0,
>        "UserPolicySizeQuota": 2048,
>        "PolicyVersionsInUse": 27,
>        "ServerCertificates": 0,
>        "Roles": 20,
>        "RolesQuota": 1000,
>        "SigningCertificatesPerUserQuota": 2,
>        "MFADevicesInUse": 0,
>        "RolePolicySizeQuota": 10240,
>        "AttachedPoliciesPerRoleQuota": 10,
>        "AccountAccessKeysPresent": 0,
>        "GroupsQuota": 300
>    }
>}
># =====================================
>```

Listing IAM identities
>``` shell
>kali@kali:~$ aws --profile target iam list-users | tee  users.json
>
># ========== Expected Result ==========
>{
>    "Users": [
>        {
>            "Path": "/admin/",
>            "UserName": "admin-alice",
>            "UserId": "AIDAQOMAIGYU3FWX3JOFP",
>            "Arn": "arn:aws:iam::123456789012:user/admin/admin-alice",
>        },
>...
># =====================================
>
>kali@kali:~$ aws --profile target iam list-groups | tee groups.json
>
># ========== Expected Result ==========
>{
>    "Groups": [
>        {
>            "Path": "/admin/",
>            "GroupName": "admin",
>            "GroupId": "AGPAQOMAIGYUXBR7QGLLN",
>            "Arn": "arn:aws:iam::123456789012:group/admin/admin",
>        },
>...
># =====================================
>
>kali@kali:~$ aws --profile target iam list-roles | tee roles.json
>
># ========== Expected Result ==========
>{
>    "Roles": [
>        {
>            "Path": "/",
>            "RoleName": "aws-controltower-AdministratorExecutionRole",
>            "RoleId": "AROAQOMAIGYU6PUFJYD7W",
>            "Arn": "arn:aws:iam::123456789012:role/aws-controltower-AdministratorExecutionRole",
>...
># =====================================
>```

Listing policies
>``` shell
>kali@kali:~$ aws --profile target iam list-policies --scope Local --only-attached | tee policies.json
>
># ========== Expected Result ==========
>{
>    "Policies": [
>        {
>            "PolicyName": "manage-credentials",
>            "PolicyId": "ANPAQOMAIGYU3LK3BHLGL",
>            "Arn": "arn:aws:iam::123456789012:policy/manage-credentials",
>            "Path": "/",
>            "DefaultVersionId": "v1",
>            "AttachmentCount": 1,
>            "PermissionsBoundaryUsageCount": 0,
>            "IsAttachable": true,
>            "UpdateDate": "2023-10-19T15:45:59+00:00"
>        },
>...
># =====================================
>```

Retrieving a snapshot of the IAM configuration with the get-account-authorization-details subcommand
>``` shell
>kali@kali:~$ aws --profile target iam get-account-authorization-details --filter User Group LocalManagedPolicy Role | tee account-authorization-details.json
>
># ========== Expected Result ==========
>{
>    "UserDetailList": [
>        {
>            "Path": "/admin/",
>            "UserName": "admin-alice",
>            "UserId": "AIDAQOMAIGYU3FWX3JOFP",
>            "Arn": "arn:aws:iam::123456789012:user/admin/admin-alice",
>            "GroupList": [
>                "amethyst_admin",
>                "admin"
>            ],
>    ...
>    "GroupDetailList": [
>        {
>            "Path": "/admin/",
>            "GroupName": "admin",
>            "GroupId": "AGPAQOMAIGYUXBR7QGLLN",
>            "Arn": "arn:aws:iam::123456789012:group/admin/admin",
>            "GroupPolicyList": [],
>    ...
>    "RoleDetailList": [
>        {
>            "Path": "/",
>            "RoleName": "aws-controltower-AdministratorExecutionRole",
>            "RoleId": "AROAQOMAIGYU6PUFJYD7W",
>            "Arn": "arn:aws:iam::123456789012:role/aws-controltower-AdministratorExecutionRole",
>    ...
>    "Policies": [
>        {
>            "PolicyName": "ruby_admin",
>            "PolicyId": "ANPAQOMAIGYU3I3WDCID3",
>            "Arn": "arn:aws:iam::123456789012:policy/ruby/ruby_admin",
>            "Path": "/ruby/",
>...
># =====================================
>```

Listing the managed policies of the clouddesk-plove IAM user
>``` shell
>kali@kali:~$ aws --profile target iam list-attached-user-policies --user-name clouddesk-plove
>
># ========== Expected Result ==========
>{
>    "AttachedPolicies": [
>        {
>            "PolicyName": "deny_challenges_access",
>            "PolicyArn": "arn:aws:iam::12345678912:policy/deny_challenges_access"
>        }
>    ]
>}
># =====================================
>```

Getting an AccessDenied error when trying to list the policy versions of the deny_challenges_access policy
>``` shell
>kali@kali:~$ aws --profile target iam list-policy-versions --policy-arn arn:aws:iam::12345678912:policy/deny_challenges_access
>
># ========== Expected Result ==========
>An error occurred (AccessDenied) when calling the ListPolicyVersions operation: User: arn:aws:iam::12345678912:user/support/clouddesk-plove is not authorized to perform: iam:ListPolicyVersions on resource: policy arn:aws:iam::12345678912:policy/deny_challenges_access with an explicit deny in an identity-based policy
># =====================================
>```

Getting the list of policy versions from the output of the get-account-authorization-details command
>``` shell
>kali@kali:~$ aws --profile target iam get-account-authorization-details --filter LocalManagedPolicy
>
># ========== Expected Result ==========
>...
>        {
>            "PolicyName": "deny_challenges_access",
>            "PolicyId": "ANPATV2ULYL4RBGWQT5SE",
>            "Arn": "arn:aws:iam::253043131129:policy/deny_challenges_access",
>            "Path": "/",
>            "DefaultVersionId": "v1",
>            "AttachmentCount": 1,
>            "PermissionsBoundaryUsageCount": 0,
>            "IsAttachable": true,
>            "CreateDate": "2023-12-11T23:25:03+00:00",
>            "UpdateDate": "2023-12-11T23:25:03+00:00",
>            "PolicyVersionList": [
>                {
>                    "Document": {
>                        "Statement": [
>                            {
>                                "Action": "*",
>                                "Condition": {
>                                    "StringEquals": {
>                                        "aws:ResourceTag/challenge": "true"
>                                    }
>                                },
>                                "Effect": "Deny",
>                                "Resource": "*",
>                                "Sid": "DenyAllIAMActionsOnChallengedResources"
>                            }
>                        ],
>                        "Version": "2012-10-17"
>                    },
>                    "VersionId": "v1",
>                    "IsDefaultVersion": true,
>                    "CreateDate": "2023-12-11T23:25:03+00:00"
>                }
>            ]
>        }
>    ]
>}
># =====================================
>```

Lab 1 - Which IAM subcommand retrieves information about IAM entity usage and IAM quotas in the Amazon Web Services account? (Write only the subcommand)
>get-account-summary

Lab 2 - Which one of the following is not a valid value for the --filter flag of the IAM get-account-authorization-detail subcommand? User, Group, Credential, Role, AWSManagedPolicy, LocalManagedPolicy?
>Credential

Lab 3 - What is the path and name of the group that the IAM user dev-ballen belongs to? (Format: /path/group_name. Example: /amethyst/admin_group)
>
