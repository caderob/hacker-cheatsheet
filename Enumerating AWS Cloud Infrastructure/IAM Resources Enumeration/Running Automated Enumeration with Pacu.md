# Running Automated Enumeration with Pacu

Installing pacu on Kali Linux using the package manager
>``` shell
>kali@kali:~$ sudo apt update
>
>kali@kali:~$ sudo apt install pacu
>```

Starting pacu in interactive mode
>``` shell
>kali@kali:~$ pacu
>
># ========== Expected Result ==========
>....
>Database created at /root/.local/share/pacu/sqlite.db
>
>What would you like to name this new session? enumlab
>Session enumlab created.
>
>...
>
>Pacu (enumlab:No Keys Set) >
># =====================================
>```

Importing the target profile credentials in pacu
>``` shell
>Pacu (enumlab:No Keys Set) > import_keys target
>
># ========== Expected Result ==========
>  Imported keys as "imported-target"
>Pacu (enumlab:imported-target) > 
># =====================================
>```

Listing all Modules in Pacu
>``` shell
>Pacu (enumlab:imported-target) > ls
>
># ========== Expected Result ==========
>...
>[Category: ENUM]
>
>  enum__secrets
>  codebuild__enum
>  ecs__enum
>  dynamodb__enum
>  aws__enum_spend
>  iam__enum_permissions
>  aws__enum_account
>  route53__enum
>  ec2__download_userdata
>  lightsail__enum
>  ecs__enum_task_def
>  ecr__enum
>  rds__enum
>  ebs__enum_volumes_snapshots
>  cloudformation__download_data
>  inspector__get_reports
>  guardduty__list_findings
>  guardduty__list_accounts
>  iam__detect_honeytokens
>  iam__bruteforce_permissions
>  lambda__enum
>  apigateway__enum
>  ec2__check_termination_protection
>  iam__enum_users_roles_policies_groups
>  ec2__enum
>  iam__get_credential_report
>  glue__enum
>  acm__enum
>  systemsmanager__download_parameters
>...
># =====================================
>```

Getting usage help for a Module in Pacu
>``` shell
>Pacu (enumlab:imported-target) > help iam__enum_users_roles_policies_groups
>
># ========== Expected Result ==========
>iam__enum_users_roles_policies_groups written by Spencer Gietzen of Rhino Security Labs.
>
>usage: pacu [--users] [--roles] [--policies] [--groups]
>
>This module requests the info for all users, roles, customer-managed
>policies, and groups in the account. If no arguments are supplied, it
>will enumerate all four, if any are supplied, it will enumerate those
>only.
>
>options:
>  --users     Enumerate info for users in the account
>  --roles     Enumerate info for roles in the account
>  --policies  Enumerate info for policies in the account
>  --groups    Enumerate info for groups in the account
># =====================================
>```

Running the iam__enum_users_roles_policies_groups Module in Pacu
>``` shell
>Pacu (enumlab:imported-target) > run iam__enum_users_roles_policies_groups
>
># ========== Expected Result ==========
>  Running module iam__enum_users_roles_policies_groups...
>[iam__enum_users_roles_policies_groups] Found 18 users
>[iam__enum_users_roles_policies_groups] Found 20 roles
>[iam__enum_users_roles_policies_groups] Found 8 policies
>[iam__enum_users_roles_policies_groups] Found 8 groups
>[iam__enum_users_roles_policies_groups] iam__enum_users_roles_policies_groups completed.
>
>[iam__enum_users_roles_policies_groups] MODULE SUMMARY:
>
>  18 Users Enumerated
>  20 Roles Enumerated
>  8 Policies Enumerated
>  8 Groups Enumerated
>  IAM resources saved in Pacu database.
># =====================================
>```

Reviewing the data collected in Pacu
>``` shell
>Pacu (enumlab:imported-target) > services
>
># ========== Expected Result ==========
>  IAM
># =====================================
>
>Pacu (enumlab:imported-target) > data IAM
>
># ========== Expected Result ==========
>{
>  "Groups": [
>    {
>      "Arn": "arn:aws:iam::123456789012:group/admin/admin",
>      "GroupId": "AGPAQOMAIGYUZQMC6G5NM",
>      "GroupName": "admin",
>      "Path": "/admin/"
>    },
>    {
>      "Arn": "arn:aws:iam::123456789012:group/amethyst/amethyst_admin",
>      "GroupId": "AGPAQOMAIGYUYF3JD3FXV",
>      "GroupName": "amethyst_admin",
>      "Path": "/amethyst/"
>    },
>...
># =====================================
>```

Lab 1 - Which option can you use with the iam__bruteforce_permissions module to specify which AWS services to target?
>B) --services

Lab 2 - Which Pacu command will let us change the currently active AWS key to another key that has previously been set for this session? (Write only the name of the command. Example: import_keys)
>
