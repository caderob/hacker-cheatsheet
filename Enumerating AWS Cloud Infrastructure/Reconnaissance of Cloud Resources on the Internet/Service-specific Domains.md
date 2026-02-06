# Service-specific Domains

Offseclab's Website
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Service-specific-Domains-1.png)

Opening the Developer Tools in Firefox
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Service-specific-Domains-2.png)

Getting the URL of the S3 Object
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Service-specific-Domains-3.gif)

Analyzing the S3 URL
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Service-specific-Domains-4.png)

List the offseclab-assets-public Bucket
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Service-specific-Domains-5.png)

List the offseclab-assets-dev Bucket
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Service-specific-Domains-6.png)

List the offseclab-assets-private Bucket
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Service-specific-Domains-7.png)

Custom URLs of All the Three Major CSPs
>``` shell
>AWS 	            | Azure 	              | GCP
>-----------------------------------------------------------------
>s3.amazonaws.com | web.core.windows.net 	| appspot.com
>awsapps.com 	    | file.core.windows.net |	storage.googleapis.com
>	                | blob.core.windows.net |	
>                 |	azurewebsites.net 	  |
>                 |	cloudapp.net 	        |
>```

Updating the Packages and Installing cloud-enum in Kali Linux
>``` shell
>kali@kali:~$ sudo apt update
>
># ========== Expected Result ==========
>[sudo] password for kali:
>...
># =====================================
>
>kali@kali:~$ sudo apt install cloud-enum
>
># ========== Expected Result ==========
>[sudo] password for kali:
>Reading package lists... Done
>Building dependency tree... Done
>Reading state information... Done
>...
>Unpacking cloud-enum (0.7-3) over (0.7-2) ...
>Setting up cloud-enum (0.7-3) ...
>Processing triggers for man-db (2.11.2-2) ...
>Processing triggers for kali-menu (2023.1.7) ...
># =====================================
>```

Getting the cloud_enum Tool Usage Options
>``` shell
>kali@kali:~$ cloud_enum --help
>
># ========== Expected Result ==========
>usage: cloud_enum [-h] (-k KEYWORD | -kf KEYFILE) [-m MUTATIONS] [-b BRUTE]
>                  [-t THREADS] [-ns NAMESERVER] [-l LOGFILE] [-f FORMAT]
>                  [--disable-aws] [--disable-azure] [--disable-gcp] [-qs]
>
>Multi-cloud enumeration utility. All hail OSINT!
>
>options:
>  -h, --help            show this help message and exit
>  -k KEYWORD, --keyword KEYWORD
>                        Keyword. Can use argument multiple times.
>  -kf KEYFILE, --keyfile KEYFILE
>                        Input file with a single keyword per line.
>  -m MUTATIONS, --mutations MUTATIONS
>                        Mutations. Default: /usr/lib/cloud-
>                        enum/enum_tools/fuzz.txt
>  -b BRUTE, --brute BRUTE
>                        List to brute-force Azure container names. Default:
>                        /usr/lib/cloud-enum/enum_tools/fuzz.txt
>  -t THREADS, --threads THREADS
>                        Threads for HTTP brute-force. Default = 5
>  -ns NAMESERVER, --nameserver NAMESERVER
>                        DNS server to use in brute-force.
>  -l LOGFILE, --logfile LOGFILE
>                        Appends found items to specified file.
>  -f FORMAT, --format FORMAT
>                        Format for log file (text,json,csv) - default: text
>  --disable-aws         Disable Amazon checks.
>  --disable-azure       Disable Azure checks.
>  --disable-gcp         Disable Google checks.
>  -qs, --quickscan      Disable all mutations and second-level scans
># =====================================
>```

Running Quick Scan Against offseclab-assets-public-axevtewi Bucket Using cloud_enum in AWS
>``` shell
>kali@kali:~$ cloud_enum -k offseclab-assets-public-axevtewi --quickscan --disable-azure --disable-gcp
>
># ========== Expected Result ==========
>...
>
>Keywords:    offseclab-assets-public-axevtewi
>Mutations:   NONE! (Using quickscan)
>Brute-list:  /usr/lib/cloud-enum/enum_tools/fuzz.txt
>
>[+] Mutated results: 1 items
>
>++++++++++++++++++++++++++
>      amazon checks
>++++++++++++++++++++++++++
>
>[+] Checking for S3 buckets
>  OPEN S3 BUCKET: http://offseclab-assets-public-axevtewi.s3.amazonaws.com/
>      FILES:
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/offseclab-assets-public-axevtewi
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/amethyst-expanded.png
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/amethyst.png
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/logo.svg
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/pic02.jpg
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/pic05.jpg
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/pic13.jpg
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/ruby-expanded.png
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/ruby.jpg
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/saphire-expanded.png
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/saphire.jpg
>                            
>                            
> Elapsed time: 00:00:00
>
>[+] Checking for AWS Apps
>[*] Brute-forcing a list of 1 possible DNS names
>                            
> Elapsed time: 00:00:00
>
>
>[+] All done, happy hacking!
># =====================================
>```

Making a Dictionary of Keywords to Search S3 Buckets
>``` shell
>kali@kali:~$ for key in "public" "private" "dev" "prod" "development" "production"; do echo "offseclab-assets-$key-axevtewi"; done | tee /tmp/keyfile.txt
>
># ========== Expected Result ==========
>offseclab-assets-public-axevtewi
>offseclab-assets-private-axevtewi
>offseclab-assets-dev-axevtewi
>offseclab-assets-prod-axevtewi
>offseclab-assets-development-axevtewi
>offseclab-assets-production-axevtewi
># =====================================
>```

Running cloud_enum Against The Generated keyfile.txt File
>``` shell
>kali@kali:~$ cloud_enum -kf /tmp/keyfile.txt -qs --disable-azure --disable-gcp
>
># ========== Expected Result ==========
>...
>
>Keywords:    offseclab-assets-public-axevtewi, offseclab-assets-private-axevtewi, offseclab-assets-dev-axevtewi, offseclab-assets-prod-axevtewi, offseclab-assets-development-axevtewi, offseclab-assets-production-axevtewi
>Mutations:   NONE! (Using quickscan)
>Brute-list:  /usr/lib/cloud-enum/enum_tools/fuzz.txt
>
>[+] Mutated results: 6 items
>
>++++++++++++++++++++++++++
>      amazon checks
>++++++++++++++++++++++++++
>
>[+] Checking for S3 buckets
>  OPEN S3 BUCKET: http://offseclab-assets-public-axevtewi.s3.amazonaws.com/
>      FILES:
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/offseclab-assets-public-axevtewi
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/amethyst-expanded.png
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/amethyst.png
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/logo.svg
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/pic02.jpg
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/pic05.jpg
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/pic13.jpg
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/ruby-expanded.png
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/ruby.jpg
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/saphire-expanded.png
>      ->http://offseclab-assets-public-axevtewi.s3.amazonaws.com/sites/www/images/saphire.jpg
>  Protected S3 Bucket: http://offseclab-assets-private-axevtewi.s3.amazonaws.com/
>                            
> Elapsed time: 00:00:06
>
>[+] Checking for AWS Apps
>[*] Brute-forcing a list of 6 possible DNS names
>                            
> Elapsed time: 00:00:00
>
>
>[+] All done, happy hacking!
>
># =====================================
>```

Lab 1 - What does the XML response indicate when received after removing the object key from the S3 URL?
>B) The bucket is publicly accessible and lists its contents

Lab 2 - Which custom URL is used by AWS for storing objects in S3 buckets?
>B) s3.amazonaws.com

Lab 3 - Use the concepts we've learned to find other S3 buckets. We may want to build a dictionary around gemstones' names as it is the theme that the target uses to name the projects. Assume that the format follows the pattern offseclab-[gemstone]-[lab_assigned_random_value]. The proof resides in an object named proof.txt.
>
