# Processing API Response data with JMESPath

Running get-account-authorization-details subcommand to get all IAM users information
>``` shell
>kali@kali:~$ aws --profile target iam get-account-authorization-details --filter User
>
># ========== Expected Result ==========
>{
>    "UserDetailList": [
>        {
>            "Path": "/admin/",
>            "UserName": "admin-alice",
>            "UserId": "AIDAQOMAIGYUSSOCFCREC",
>            "Arn": "arn:aws:iam::123456789012:user/admin/admin-alice",
>            "GroupList": [
>                "admin"
>            ],
>            "AttachedManagedPolicies": [],
>            "Tags": []
>        },
>        {
>            "Path": "/amethyst/",
>            "UserName": "admin-cbarton",
>            "UserId": "AIDAQOMAIGYUTHT4D5YLG",
>            "Arn": "arn:aws:iam::123456789012:user/amethyst/admin-cbarton",
>            "GroupList": [
>                "amethyst_admin"
>            ],
>            "AttachedManagedPolicies": [],
>            "Tags": []
>        },
>...
># =====================================
>```

Querying the UserName key from the JSON document
>``` shell
>kali@kali:~$ aws --profile target iam get-account-authorization-details --filter User --query "UserDetailList[].UserName"
>
># ========== Expected Result ==========
>[
>    "admin-alice",
>    "admin-cbarton",
>    "admin-srogers",
>    "admin-tstark",
>    "clouddesk-bob",
>...
># =====================================
>```

Querying for more than one key values
>``` shell
>kali@kali:~$ aws --profile target iam get-account-authorization-details --filter User --query "UserDetailList[0].[UserName,Path,GroupList]"
>
># ========== Expected Result ==========
>[
>    "admin-alice",
>    "/admin/",
>    [
>        "admin"
>    ]
>]
># =====================================
>
>kali@kali:~$ aws --profile target iam get-account-authorization-details --filter User --query "UserDetailList[0].{Name: UserName,Path: Path,Groups: GroupList}"
>
># ========== Expected Result ==========
>{
>    "Name": "admin-alice",
>    "Path": "/admin/",
>    "Groups": [
>        "admin"
>    ]
>}
># =====================================
>```

Filtering all IAM Users whose names contain admin
>``` shell
>kali@kali:~$ aws --profile target iam get-account-authorization-details --filter User --query "UserDetailList[?contains(UserName, 'admin')].{Name: UserName}"
>
># ========== Expected Result ==========
>[
>    {
>        "Name": "admin-alice"
>    },
>    {
>        "Name": "admin-cbarton"
>    },
>    {
>        "Name": "admin-srogers"
>    },
>
>...
># =====================================
>```

Constructing more advanced queries
>``` shell
>kali@kali:~$ aws --profile target iam get-account-authorization-details --filter User Group --query "{Users: UserDetailList[?Path=='/admin/'].UserName, Groups: GroupDetailList[?Path=='/admin/'].{Name: GroupName}}"
>
># ========== Expected Result ==========
>{
>   "Users": [
>       "admin-alice"
>   ],
>   "Groups": [
>       {
>           "Name": "admin"
>       }
>   ]
>}
># =====================================
>```

Lab 1 - Which argument allows you to filter data on the server side when using AWS CLI?
>B) --filter

Lab 2 - What will the JMESPath expression "UserDetailList[].UserName" retrieve?
>C) All UserName values from the UserDetailList array

Lab 3 - What JMESPath expression will filter and display all users that contain the word "admin" in the Username and the Path fields? (Write only the JMESPath expression starting with "?". Use the contains function for both conditions. Example: ?contains(Path,'admin') ... )
>?contains(UserName,'admin') && contains(Path,'admin')
