# DB Types and Characteristics

Connecting to the remote MySQL instance
>``` shell
>kali@kali:~$ mysql -u root -p'root' -h 192.168.50.16 -P 3306 --skip-ssl-verify-server-cert
>
># ========== Expected Result ==========
>Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.
>
>Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
>
>MySQL [(none)]>
># =====================================
>```

Retrieving the version of a MySQL database
>``` shell
>MySQL [(none)]> select version();
>
># ========== Expected Result ==========
>+-----------+
>| version() |
>+-----------+
>| 8.0.21    |
>+-----------+
>1 row in set (0.107 sec)
># =====================================
>```

Inspecting the current session's user
>``` shell
>MySQL [(none)]> select system_user();
>
># ========== Expected Result ==========
>+--------------------+
>| system_user()      |
>+--------------------+
>| root@192.168.20.50 |
>+--------------------+
>1 row in set (0.104 sec)
># =====================================
>```

Listing all Available Databases
>``` shell
>MySQL [(none)]> show databases;
>
># ========== Expected Result ==========
>+--------------------+
>| Database           |
>+--------------------+
>| information_schema |
>| mysql              |
>| performance_schema |
>| sys                |
>| test               |
>+--------------------+
>5 rows in set (0.107 sec)
># =====================================
>```

Inspecting user's encrypted password
>``` shell
>MySQL [(none)]> SELECT user, authentication_string FROM mysql.user WHERE user = 'offsec';
>
># ========== Expected Result ==========
>+--------+------------------------------------------------------------------------+
>| user   | authentication_string                                                  |
>+--------+------------------------------------------------------------------------+
>| offsec | $A$005$?qvorPp8#lTKH1j54xuw4C5VsXe5IAa1cFUYdQMiBxQVEzZG9XWd/e6|
>+--------+------------------------------------------------------------------------+
>1 row in set (0.106 sec)
># =====================================
>```

Connecting to the Remote MSSQL instance via Impacket
>``` shell
>kali@kali:~$ impacket-mssqlclient Administrator:Lab123@192.168.50.18 -windows-auth
>
># ========== Expected Result ==========
>Impacket v0.9.24 - Copyright 2021 SecureAuth Corporation
>
>[*] Encryption required, switching to TLS
>[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
>[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
>[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
>[*] INFO(SQL01\SQLEXPRESS): Line 1: Changed database context to 'master'.
>[*] INFO(SQL01\SQLEXPRESS): Line 1: Changed language setting to us_english.
>[*] ACK: Result: 1 - Microsoft SQL Server (150 7208)
>[!] Press help for extra shell commands
>SQL (SQLPLAYGROUND\Administrator  dbo@master)>
># =====================================
>```

Retrieving the Windows OS Version
>``` shell
>SQL (SQLPLAYGROUND\Administrator  dbo@master)> SELECT @@version;
>
># ========== Expected Result ==========
>...
>
>Microsoft SQL Server 2019 (RTM) - 15.0.2000.5 (X64)
>	Sep 24 2019 13:48:23
>	Copyright (C) 2019 Microsoft Corporation
>	Express Edition (64-bit) on Windows Server 2022 Standard 10.0 <X64> (Build 20348: ) (Hypervisor)
># =====================================
>```

Inspecting the Available Databases
>``` shell
>SQL (SQLPLAYGROUND\Administrator  dbo@master)> SELECT name FROM sys.databases;
>
># ========== Expected Result ==========
>name
>...
>master
>
>tempdb
>
>model
>
>msdb
>
>offsec
>
>SQL>
># =====================================
>```

Inspecting the Available Tables in the offsec Database
>``` shell
>SQL (SQLPLAYGROUND\Administrator  dbo@master)> SELECT * FROM offsec.information_schema.tables;
>
># ========== Expected Result ==========
>TABLE_CATALOG   TABLE_SCHEMA   TABLE_NAME   TABLE_TYPE   
>-------------   ------------   ----------   ----------   
>offsec          dbo            users        b'BASE TABLE'   
>
>SQL (SQLPLAYGROUND\Administrator  dbo@master)> 
># =====================================
>```

Exploring Users Table Records
>``` shell
>SQL>select * from offsec.dbo.users;
>
># ========== Expected Result ==========
>username     password     
>----------   ----------   
>admin        lab        
>
>guest        guest 
># =====================================
>```

Lab 1 - From your Kali Linux VM, connect to the remote MySQL instance on VM 1 and replicate the steps to enumerate the MySQL database. Then explore all values assigned to the user offsec. Which plugin value is used as a password authentication scheme?
>``` shell
>
>```
>caching_sha2_password

Lab 2 - From your Kali Linux VM, connect to the remote MSSQL instance on VM 2 and replicate the steps to enumerate the MSSQL database. Then explore the records of the sysusers table inside the master database. What is the value of the first user listed?
>``` shell
>
>```
>public

Lab 3 - From your Kali Linux VM, connect to the remote MySQL instance on VM 3 and explore the users table present in one of the databases to get the flag.
>``` shell
>
>```
>OS{2a23dbaae2da3e8502feed2d8bf0da3e}
