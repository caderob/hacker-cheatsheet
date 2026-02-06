# Examining Compromised Credentials

Configuring AWS CLI with profiles
>``` shell
>kali@kali:~$ aws configure --profile target
>
># ========== Expected Result ==========
>AWS Access Key ID []: AKIAVXWRNA7HUYFERLHS...
>AWS Secret Access Key []: u1CmqAO9QR...
>Default region name []: us-east-1
>Default output format []: json
># =====================================
>```

Getting details from the compromised credentials by running get-caller-identity
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

Getting the account ID from access keys with the get-access-key-info command
>``` shell
>kali@kali:~$ kali@kali:~$ aws configure --profile challenge
>
># ========== Expected Result ==========
>AWS Access Key ID []: AKIAVXW...
>AWS Secret Access Key []: KlnPvlFhvrrxg...
>Default region name []: us-east-1
>Default output format []: json
># =====================================
>
>kali@kali:~$ aws --profile challenge sts get-access-key-info --access-key-id AKIAQOMAIGYUVEHJ7WXM
>
># ========== Expected Result ==========
>{
>    "Account": "123456789012"
>}
># =====================================
>```

Getting information from error messages
>``` shell
>kali@kali:~$ aws --profile target lambda invoke --function-name arn:aws:lambda:us-east-1:123456789012:function:nonexistent-function outfile
>
># ========== Expected Result ==========
>An error occurred (AccessDeniedException) when calling the Invoke operation: User: arn:aws:iam::123456789012:user/support/clouddesk-plove is not authorized to perform: lambda:InvokeFunction on resource: arn:aws:lambda:us-east-1:123456789012:function:nonexistent-function because no resource-based policy allows the lambda:InvokeFunction action
># =====================================
>```

Navigating to Cloudtrail Event History
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Examining-Compromised-Credentials-1.gif)

Executing an API request to another region
>``` shell
>kali@kali:~$ aws --profile target sts get-caller-identity --region us-east-2
>
># ========== Expected Result ==========
>{
>    "UserId": "AIDAQOMAIGYUYVDBXFNVF",
>    "Account": "123456789012",
>    "Arn": "arn:aws:iam::123456789012:user/support/clouddesk-plove"
>}
># =====================================
>```

Comparing CloudTrail Events Between Regions in the AWS Management Console
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Examining-Compromised-Credentials-2.gif)

Lab 1 - In AWS CLI, what sts subcommand returns details about the IAM user or role whose credentials are used to call the operation? (Write only the name of the subcommand)
>get-caller-identity

Lab 2 - In AWS CLI, what sts subcommand returns the account identifier for the specified access key ID? (Write only the name of the subcommand)
>get-access-key-info

Lab 3 - In AWS CLI, what is the name of the option flag that specifies the region to use overlapping the default region? (Write your answer in this format: --name)
>--region
