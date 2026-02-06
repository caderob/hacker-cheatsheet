# Publicly Shared Resources

Listing All Public AMIs Owned by Amazon AWS
>``` shell
>kali@kali:~$ aws --profile attacker ec2 describe-images --owners amazon --executable-users all
>
># ========== Expected Result ==========
>{
>    "Images": [
>        {
>            "Architecture": "x86_64",
>            "CreationDate": "2022-06-29T09:46:55.000Z",
>            "ImageId": "ami-0d4f490f4e62171b4",
>            "ImageLocation": "amazon/Deep Learning Base AMI (Amazon Linux 2) Version 53.4",
>            "ImageType": "machine",
>            "Public": true,
>            "OwnerId": "898082745236",
>            "PlatformDetails": "Linux/UNIX",
>            "UsageOperation": "RunInstances",
>            "State": "available",
>            "BlockDeviceMappings": [
>                {
>                    "DeviceName": "/dev/xvda",
>                    "Ebs": {
>                        "DeleteOnTermination": true,
>                        "Iops": 3000,
>                        "SnapshotId": "snap-0ce7f231ea72dd0ea",
>                        "VolumeSize": 100,
>...
># =====================================
>```

The Filter Expression Format
>``` shell
>--filters "Name=filter-name,Values=filter-value1,filter-value2,..."
>```

The Filter Expression Format for offseclab Word
>``` shell
>   --filters "Name=description,Values=*Offseclab*"
>```

Listing All Public AMIs After Filtering the List Using the Keyword "description"
>``` shell
>kali@kali:~$ aws --profile attacker ec2 describe-images --executable-users all --filters "Name=description,Values=*Offseclab*"
>
># ========== Expected Result ==========
>{
>    "Images": []
>}
># =====================================
>```

Listing All Public AMIs After Filtering the List Using the Keyword "name"
>``` shell
>kali@kali:~$ aws --profile attacker ec2 describe-images --executable-users all --filters "Name=name,Values=*Offseclab*"
>
># ========== Expected Result ==========
>{
>    "Images": [
>        {
>            "Architecture": "x86_64",
>            "CreationDate": "2023-08-05T19:43:29.000Z",
>            "ImageId": "ami-0854d94958c0a17e6",
>            "ImageLocation": "123456789012/Offseclab Base AMI",
>            "ImageType": "machine",
>            "Public": true,
>            "OwnerId": "123456789012",
>            "PlatformDetails": "Linux/UNIX",
>            "UsageOperation": "RunInstances",
>            "State": "available",
>            "BlockDeviceMappings": [
>                {
>                    "DeviceName": "/dev/xvda",
>                    "Ebs": {
>                        "DeleteOnTermination": true,
>                {
>                    "DeviceName": "/dev/xvda",
>                    "Ebs": {
>                        "DeleteOnTermination": true,
>                        "DeleteOnTermination": true,
>                        "SnapshotId": "snap-098dc18c797e4f255",
>                        "VolumeSize": 8,
>                        "VolumeType": "gp2",
>                        "Encrypted": false
>                    }
>                }
>            ],
>            "EnaSupport": true,
>            "Hypervisor": "xen",
>            "Name": "Offseclab Base AMI",
>            "RootDeviceName": "/dev/xvda",
>            "RootDeviceType": "ebs",
>            "SriovNetSupport": "simple",
>            "Tags": [
>                {
>                    "Key": "Name",
>                    "Value": "Offseclab Base AMI"
>                }
>            ],
>            "VirtualizationType": "hvm",
>            "DeprecationTime": "2023-08-05T21:43:00.000Z"
>        }
>    ]
>}
># =====================================
>```

Listing Public Snapshots After Filtering the List Using the Keyword "description"
>``` shell
>kali@kali:~$ aws --profile attacker ec2 describe-snapshots --filters "Name=description,Values=*offseclab*"
>
># ========== Expected Result ==========
>{
>    "Snapshots": []
>}
># =====================================
>```

Lab 1 - Why might large organizations share cloud resources publicly or between accounts?
>B) To facilitate internal operations and resource sharing

Lab 2 - What is the purpose of the --owners amazon argument in the AWS CLI command?
>C) To list all images owned by AWS

Lab 3 - Use the attacker's account ID to search for other publicly shared resources. You will find a 1 GB-sized snapshot (VoumeSize: 1). Copy the description of the newly found resource and paste it into the answer box. (This resource is not really publicly shared, but we should be able to list it with the provided credentials for the lab.)
>
