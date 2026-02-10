# WEBSRV1

Nmap scan of WEBSRV1
>``` shell
>kali@kali:~/beyond$ sudo nmap -sC -sV -oN websrv1/nmap 192.168.50.244
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-09-29 11:18 EDT
>Nmap scan report for 192.168.50.244
>Host is up (0.11s latency).
>Not shown: 998 closed tcp ports (reset)
>PORT   STATE SERVICE VERSION
>22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3 (Ubuntu Linux; protocol 2.0)
>| ssh-hostkey: 
>|   256 4f:c8:5e:cd:62:a0:78:b4:6e:d8:dd:0e:0b:8b:3a:4c (ECDSA)
>|_  256 8d:6d:ff:a4:98:57:82:95:32:82:64:53:b2:d7:be:44 (ED25519)
>80/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
>| http-title: BEYOND Finances &#8211; We provide financial freedom
>|_Requested resource was http://192.168.50.244/main/
>|_http-server-header: Apache/2.4.52 (Ubuntu)
>|_http-generator: WordPress 6.0.2
>Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
>
>Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
>Nmap done: 1 IP address (1 host up) scanned in 19.51 seconds
># =====================================
>```

OpenSSH versions in Jammy Jellyfish
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/WEBSRV1-1.png)

Landing Page of WEBSRV1 (1)
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/WEBSRV1-2.png)

Landing Page of WEBSRV1 (2)
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/WEBSRV1-3.png)

WhatWeb scan of WEBSRV1
>``` shell
>kali@kali:~/beyond$ whatweb http://192.168.50.244
>
># ========== Expected Result ==========
>http://192.168.50.244 [301 Moved Permanently] Apache[2.4.52], Country[RESERVED][ZZ], HTTPServer[Ubuntu Linux][Apache/2.4.52 (Ubuntu)], IP[192.168.50.244], RedirectLocation[http://192.168.50.244/main/], UncommonHeaders[x-redirect-by]
>http://192.168.50.244/main/ [200 OK] Apache[2.4.52], Country[RESERVED][ZZ], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.52 (Ubuntu)], IP[192.168.50.244], JQuery[3.6.0], MetaGenerator[WordPress 6.0.2], Script, Title[BEYOND Finances &#8211; We provide financial freedom], UncommonHeaders[link], WordPress[6.0.2]
># =====================================
>```

WPScan of the WordPress web page on WEBSRV1
>``` shell
>kali@kali:~/beyond$ wpscan --url http://192.168.50.244 --enumerate p --plugins-detection aggressive -o websrv1/wpscan
>
>kali@kali:~/beyond$ cat websrv1/wpscan
>
># ========== Expected Result ==========
>...
>
>[i] Plugin(s) Identified:
>
>[+] akismet
> | Location: http://192.168.50.244/wp-content/plugins/akismet/
> | Latest Version: 5.0
> | Last Updated: 2022-07-26T16:13:00.000Z
> |
> | Found By: Known Locations (Aggressive Detection)
> |  - http://192.168.50.244/wp-content/plugins/akismet/, status: 500
> |
> | The version could not be determined.
>
>[+] classic-editor
> | Location: http://192.168.50.244/wp-content/plugins/classic-editor/
> | Latest Version: 1.6.2 
> | Last Updated: 2021-07-21T22:08:00.000Z
>...
>
>[+] contact-form-7
> | Location: http://192.168.50.244/wp-content/plugins/contact-form-7/
> | Latest Version: 5.6.3 (up to date)
> | Last Updated: 2022-09-01T08:48:00.000Z
>...
>
>[+] duplicator
> | Location: http://192.168.50.244/wp-content/plugins/duplicator/
> | Last Updated: 2022-09-24T17:57:00.000Z
> | Readme: http://192.168.50.244/wp-content/plugins/duplicator/readme.txt
> | [!] The version is out of date, the latest version is 1.5.1
> |
> | Found By: Known Locations (Aggressive Detection)
> |  - http://192.168.50.244/wp-content/plugins/duplicator/, status: 403
> |
> | Version: 1.3.26 (80% confidence)
> | Found By: Readme - Stable Tag (Aggressive Detection)
> |  - http://192.168.50.244/wp-content/plugins/duplicator/readme.txt
>
>[+] elementor
> | Location: http://192.168.50.244/wp-content/plugins/elementor/
> | Latest Version: 3.7.7 (up to date)
> | Last Updated: 2022-09-20T14:51:00.000Z
>...
>
>[+] wordpress-seo
> | Location: http://192.168.50.244/wp-content/plugins/wordpress-seo/
> | Latest Version: 19.7.1 (up to date)
> | Last Updated: 2022-09-20T14:10:00.000Z
>...
># =====================================
>```

SearchSploit results for the Duplicator WordPress plugin
>``` shell
>kali@kali:~/beyond$ searchsploit duplicator 
>
># ========== Expected Result ==========
>-------------------------------------------------------------------------------------- ---------------------------------
> Exploit Title                                                                        |  Path
>-------------------------------------------------------------------------------------- ---------------------------------
>WordPress Plugin Duplicator - Cross-Site Scripting                                    | php/webapps/38676.txt
>WordPress Plugin Duplicator 0.5.14 - SQL Injection / Cross-Site Request Forgery       | php/webapps/36735.txt
>WordPress Plugin Duplicator 0.5.8 - Privilege Escalation                              | php/webapps/36112.txt
>WordPress Plugin Duplicator 1.2.32 - Cross-Site Scripting                             | php/webapps/44288.txt
>Wordpress Plugin Duplicator 1.3.26 - Unauthenticated Arbitrary File Read              | php/webapps/50420.py
>Wordpress Plugin Duplicator 1.3.26 - Unauthenticated Arbitrary File Read (Metasploit) | php/webapps/49288.rb
>WordPress Plugin Duplicator 1.4.6 - Unauthenticated Backup Download                   | php/webapps/50992.txt
>WordPress Plugin Duplicator 1.4.7 - Information Disclosure                            | php/webapps/50993.txt
>WordPress Plugin Multisite Post Duplicator 0.9.5.1 - Cross-Site Request Forgery       | php/webapps/40908.html
>-------------------------------------------------------------------------------------- ---------------------------------
>Shellcodes: No Results
># =====================================
>```
