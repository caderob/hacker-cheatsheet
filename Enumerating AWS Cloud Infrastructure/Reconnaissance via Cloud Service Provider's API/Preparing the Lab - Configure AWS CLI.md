# Preparing the Lab - Configure AWS CLI

Installing AWS CLI in Kali Linux
>``` shell
>kali@kali:~$ sudo apt update
>
># ========== Expected Result ==========
>...
># =====================================
>
>kali@kali:~$ sudo apt install -y awscli
>
># ========== Expected Result ==========
>...
>The following NEW packages will be installed:
>  awscli docutils-common python3-awscrt python3-docutils python3-jmespath python3-roman
>(Reading database ... 461429 files and directories currently installed.)
>...
># =====================================
>```

Configuring Profile and Validating Communication with AWS API
>``` shell
>kali@kali:~$ aws configure --profile attacker
>
># ========== Expected Result ==========
>AWS Access Key ID []: AKIAQO...
>AWS Secret Access Key []: cOGzm...
>Default region name []: us-east-1
>Default output format []: json
># =====================================
>
>kali@kali:~$ aws --profile attacker sts get-caller-identity
>
># ========== Expected Result ==========
>{
>    "UserId": "AIDAQOMAIGYU5VFQCHOI4",
>    "Account": "123456789012",
>    "Arn": "arn:aws:iam::123456789012:user/attacker"
>}
># =====================================
>```
