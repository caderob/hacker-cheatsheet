# Using Non-Executable Files

Mountain Desserts Application on Windows
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Using-Non-Executable-Files-18.png)

Failed attempts to access PHP files
>``` shell
>kali@kali:~$ curl http://mountaindesserts.com:8000/index.php
>
># ========== Expected Result ==========
>404 page not found
># =====================================
>
>kali@kali:~$ curl http://mountaindesserts.com:8000/meteor/index.php
>
># ========== Expected Result ==========
>404 page not found
># =====================================
>
>kali@kali:~$ curl http://mountaindesserts.com:8000/admin.php
>
># ========== Expected Result ==========
>404 page not found
># =====================================
>```
