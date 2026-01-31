# SSH Dynamic Port Forwarding

The SSH dynamic port forward setup
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/SSH-Dynamic-Port-Forwarding-1.png)

Opening the SSH dynamic port forward on port 9999
>``` shell
>confluence@confluence01:/opt/atlassian/confluence/bin$ python3 -c 'import pty; pty.spawn("/bin/sh")'
>
># ========== Expected Result ==========
></bin$ python3 -c 'import pty; pty.spawn("/bin/sh")'
># =====================================
>
>$ ssh -N -D 0.0.0.0:9999 database_admin@10.4.50.215
>
># ========== Expected Result ==========
><$ ssh -N -D 0.0.0.0:9999 database_admin@10.4.50.215   
>Could not create directory '/home/confluence/.ssh'.
>The authenticity of host '10.4.50.215 (10.4.50.215)' can't be established.
>ECDSA key fingerprint is SHA256:K9x2nuKxQIb/YJtyN/YmDBVQ8Kyky7tEqieIyt1ytH4.
>Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
>yes
>Failed to add the host to the list of known hosts (/home/confluence/.ssh/known_hosts).
>database_admin@10.4.50.215's password:
># =====================================
>```

The Proxychains configuration file, pointing towards the SOCKS proxy set up on CONFLUENCE01
>``` shell
>kali@kali:~$ tail /etc/proxychains4.conf
>
># ========== Expected Result ==========
>#       proxy types: http, socks4, socks5, raw
>#         * raw: The traffic is simply forwarded to the proxy without modification.
>#        ( auth types supported: "basic"-http  "user/pass"-socks )
>#
>[ProxyList]
># add proxy here ...
># meanwile
># defaults set to "tor"
>socks5 192.168.50.63 9999
># =====================================
>```

smbclient connecting to HRSHARES through the SOCKS proxy using Proxychains
>``` shell
>kali@kali:~$ proxychains smbclient -L //172.16.50.217/ -U hr_admin --password=Welcome1234
>
># ========== Expected Result ==========
>[proxychains] config file found: /etc/proxychains4.conf
>[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
>[proxychains] DLL init: proxychains-ng 4.16
>[proxychains] Strict chain  ...  192.168.50.63:9999  ...  172.16.50.217:445  ...  OK
>
>        Sharename       Type      Comment
>        ---------       ----      -------
>        ADMIN$          Disk      Remote Admin
>        C$              Disk      Default share
>        IPC$            IPC       Remote IPC
>    scripts         Disk
>        Users           Disk      
>Reconnecting with SMB1 for workgroup listing.
>[proxychains] Strict chain  ...  192.168.50.63:9999  ...  172.16.50.217:139  ...  OK
>[proxychains] Strict chain  ...  192.168.50.63:9999  ...  172.16.50.217:139  ...  OK
>do_connect: Connection to 172.16.50.217 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
>Unable to connect with SMB1 -- no workgroup available
># =====================================
>```

The Nmap-over-Proxychains command output
>``` shell
>kali@kali:~$ sudo proxychains nmap -vvv -sT --top-ports=20 -Pn 172.16.50.217
>
># ========== Expected Result ==========
>[proxychains] config file found: /etc/proxychains4.conf
>[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
>[proxychains] DLL init: proxychains-ng 4.16
>Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower.
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-08-20 17:26 EDT
>Initiating Parallel DNS resolution of 1 host. at 17:26
>Completed Parallel DNS resolution of 1 host. at 17:26, 0.09s elapsed
>DNS resolution of 1 IPs took 0.10s. Mode: Async [#: 2, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
>Initiating Connect Scan at 17:26
>Scanning 172.16.50.217 [20 ports]
>[proxychains] Strict chain  ...  192.168.50.63:9999  ...  172.16.50.217:111 <--socket error or timeout!
>[proxychains] Strict chain  ...  192.168.50.63:9999  ...  172.16.50.217:22 <--socket error or timeout!
>...
>[proxychains] Strict chain  ...  192.168.50.63:9999  ...  172.16.50.217:5900 <--socket error or timeout!
>Completed Connect Scan at 17:30, 244.33s elapsed (20 total ports)
>Nmap scan report for 172.16.50.217
>Host is up, received user-set (9.0s latency).
>Scanned at 2022-08-20 17:26:47 EDT for 244s
>
>PORT     STATE  SERVICE       REASON
>21/tcp   closed ftp           conn-refused
>22/tcp   closed ssh           conn-refused
>23/tcp   closed telnet        conn-refused
>25/tcp   closed smtp          conn-refused
>53/tcp   closed domain        conn-refused
>80/tcp   closed http          conn-refused
>110/tcp  closed pop3          conn-refused
>111/tcp  closed rpcbind       conn-refused
>135/tcp  open   msrpc         syn-ack
>139/tcp  open   netbios-ssn   syn-ack
>143/tcp  closed imap          conn-refused
>443/tcp  closed https         conn-refused
>445/tcp  open   microsoft-ds  syn-ack
>993/tcp  closed imaps         conn-refused
>995/tcp  closed pop3s         conn-refused
>1723/tcp closed pptp          conn-refused
>3306/tcp closed mysql         conn-refused
>3389/tcp open   ms-wbt-server syn-ack
>5900/tcp closed vnc           conn-refused
>8080/tcp closed http-proxy    conn-refused
>
>Read data files from: /usr/bin/../share/nmap
>Nmap done: 1 IP address (1 host up) scanned in 244.62 seconds
># =====================================
>```

Lab 1 - Follow this walkthrough, and scan HRSHARES from the Kali machine using Nmap and Proxychains. What port between 4870 and 4900 is open? (this will take several minutes to run)
>``` shell
>
>```
>

Lab 2 - Download the client binary ssh_dynamic_client from the Resources section. If you're using the aarch64 build of Kali, download the ssh_dynamic_client_aarch64 binary. Using Proxychains, run the binary from your Kali machine against the port you just found.
>``` shell
>
>```
>
