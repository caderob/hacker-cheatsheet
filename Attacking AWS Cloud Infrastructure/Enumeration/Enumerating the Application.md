# Enumerating the Application

Home Page of Application
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Enumerating-the-Application-1.png)

Running dirb Against Target
>``` shell
>kali@kali:~$ dirb http://app.offseclab.io
>
># ========== Expected Result ==========
>....
>
>GENERATED WORDS: 4612                                                          
>
>---- Scanning URL: http://app.offseclab.io/ ----
>+ http://app.offseclab.io/index.html (CODE:200|SIZE:3189)                                                                  
>...
># =====================================
>```

App HTML Source
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Enumerating-the-Application-2.png)

HTML source with S3 Bucket
>``` shell
><div class="carousel-item active">
>    <img src="https://staticcontent-lgudbhv8syu2tgbk.s3.us-east-1.amazonaws.com/images/bunny.jpg" class="d-block w-100" alt="...">
></div>
><div class="carousel-item">
>    <img src="https://staticcontent-lgudbhv8syu2tgbk.s3.us-east-1.amazonaws.com/images/golden-with-flower.jpg" class="d-block w-100"
>        alt="...">
></div>
><div class="carousel-item">
>    <img src="https://staticcontent-lgudbhv8syu2tgbk.s3.us-east-1.amazonaws.com/images/kittens.jpg" class="d-block w-100" alt="...">
></div>
><div class="carousel-item">
>    <img src="https://staticcontent-lgudbhv8syu2tgbk.s3.us-east-1.amazonaws.com/images/puppy.jpg" class="d-block w-100" alt="...">
></div>
>```

Using curl to list S3 bucket - Error
>``` shell
>kali@kali:~$ curl https://staticcontent-lgudbhv8syu2tgbk.s3.us-east-1.amazonaws.com  
>
># ========== Expected Result ==========
><?xml version="1.0" encoding="UTF-8"?>
><Error><Code>AccessDenied</Code><Message>Access Denied</Message><RequestId>VFK5KNV3PV9B8SKJ</RequestId><HostId>0J13xDMdIwQB3e3HLcQvfYpsRe1MO0Bn0OVUgl+7wtbs2v3XOZZn98WKQ0lsyqmpgnv5FjSGFaE=</HostId></Error>
># =====================================
>```

Running Enumeration on S3 Bucket
>``` shell
>kali@kali:~$ head -n 51 /usr/share/wordlists/dirb/common.txt > first50.txt
>
>kali@kali:~$ dirb https://staticcontent-lgudbhv8syu2tgbk.s3.us-east-1.amazonaws.com ./first50.txt
>
># ========== Expected Result ==========
>...
>---- Scanning URL: https://staticcontent-lgudbhv8syu2tgbk.s3.us-east-1.amazonaws.com/ ----
>+ https://staticcontent-lgudbhv8syu2tgbk.s3.us-east-1.amazonaws.com/.git/HEAD (CODE:200|SIZE:23)      
>...
>DOWNLOADED: 50 - FOUND: 1
># =====================================
>```

Configuring AWS CLI
>``` shell
>kali@kali:~$ aws configure
>
># ========== Expected Result ==========
>AWS Access Key ID [None]: AKIAUBHUBEGIBVQAI45N
>AWS Secret Access Key [None]: 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
>Default region name [None]: us-east-1
>Default output format [None]: 
># =====================================
>```

Listing Bucket
>``` shell
>kali@kali:~$ aws s3 ls staticcontent-lgudbhv8syu2tgbk
>
># ========== Expected Result ==========
>                           PRE .git/
>                           PRE images/
>                           PRE scripts/
>                           PRE webroot/
>2023-04-04 13:00:52        972 CONTRIBUTING.md
>2023-04-04 13:00:52         79 Caddyfile
>2023-04-04 13:00:52        407 Jenkinsfile
>2023-04-04 13:00:52        850 README.md
>2023-04-04 13:00:52        176 docker-compose.yml 
># =====================================
>```

Lab 1 - Discover the flag in the source of the web page.
>``` shell
>
>```
>

Lab 2 - What useful information was discovered when viewing the HTML source of the application?
>C) The use of S3 buckets for storing images

Lab 3 - Which command was used to list the contents of the S3 bucket using the AWS CLI?
>B) aws s3 ls
