# Active Information Gathering

## Port Scanning with Nmap

Scanning an IP for the 1000 most popular TCP ports
>``` shell
>kali@kali:~$ nmap 192.168.50.149
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-09 05:12 EST
>Nmap scan report for 192.168.50.149
>Host is up (0.10s latency).
>Not shown: 989 closed tcp ports (conn-refused)
>PORT     STATE SERVICE
>53/tcp   open  domain
>88/tcp   open  kerberos-sec
>135/tcp  open  msrpc
>139/tcp  open  netbios-ssn
>389/tcp  open  ldap
>445/tcp  open  microsoft-ds
>464/tcp  open  kpasswd5
>593/tcp  open  http-rpc-epmap
>636/tcp  open  ldapssl
>3268/tcp open  globalcatLDAP
>3269/tcp open  globalcatLDAPssl
>
>Nmap done: 1 IP address (1 host up) scanned in 10.95 seconds
># =====================================
>```

Using nmap to perform a SYN scan
>``` shell
>kali@kali:~$ sudo nmap -sS 192.168.50.149
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-09 06:31 EST
>Nmap scan report for 192.168.50.149
>Host is up (0.11s latency).
>Not shown: 989 closed tcp ports (reset)
>PORT     STATE SERVICE
>53/tcp   open  domain
>88/tcp   open  kerberos-sec
>135/tcp  open  msrpc
>139/tcp  open  netbios-ssn
>389/tcp  open  ldap
>445/tcp  open  microsoft-ds
>464/tcp  open  kpasswd5
>593/tcp  open  http-rpc-epmap
>636/tcp  open  ldapssl
>3268/tcp open  globalcatLDAP
>3269/tcp open  globalcatLDAPssl
>...
># =====================================
>```

Using nmap to perform a TCP connect scan
>``` shell
>kali@kali:~$ nmap -sT 192.168.50.149
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-09 06:44 EST
>Nmap scan report for 192.168.50.149
>Host is up (0.11s latency).
>Not shown: 989 closed tcp ports (conn-refused)
>PORT     STATE SERVICE
>53/tcp   open  domain
>88/tcp   open  kerberos-sec
>135/tcp  open  msrpc
>139/tcp  open  netbios-ssn
>389/tcp  open  ldap
>445/tcp  open  microsoft-ds
>464/tcp  open  kpasswd5
>593/tcp  open  http-rpc-epmap
>636/tcp  open  ldapssl
>3268/tcp open  globalcatLDAP
>3269/tcp open  globalcatLDAPssl
>...
># =====================================
>```

Using nmap to perform a UDP scan
>``` shell
>kali@kali:~$ sudo nmap -sU 192.168.50.149
>
># ========== Expected Result ==========
>Starting Nmap 7.70 ( https://nmap.org ) at 2019-03-04 11:46 EST
>Nmap scan report for 192.168.131.149
>Host is up (0.11s latency).
>Not shown: 977 closed udp ports (port-unreach)
>PORT      STATE         SERVICE
>123/udp   open          ntp
>389/udp   open          ldap
>...
>Nmap done: 1 IP address (1 host up) scanned in 22.49 seconds
># =====================================
>```

Using nmap to perform a combined UDP and SYN scan
>``` shell
>kali@kali:~$ sudo nmap -sU -sS 192.168.50.149
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-09 08:16 EST
>Nmap scan report for 192.168.50.149
>Host is up (0.10s latency).
>Not shown: 989 closed tcp ports (reset), 977 closed udp ports (port-unreach)
>PORT      STATE         SERVICE
>53/tcp    open          domain
>88/tcp    open          kerberos-sec
>135/tcp   open          msrpc
>139/tcp   open          netbios-ssn
>389/tcp   open          ldap
>445/tcp   open          microsoft-ds
>464/tcp   open          kpasswd5
>593/tcp   open          http-rpc-epmap
>636/tcp   open          ldapssl
>3268/tcp  open          globalcatLDAP
>3269/tcp  open          globalcatLDAPssl
>53/udp    open          domain
>123/udp   open          ntp
>389/udp   open          ldap
>...
># =====================================
>```

Using nmap to perform a network sweep
>``` shell
>kali@kali:~$ nmap -sn 192.168.50.1-253
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 03:19 EST
>Nmap scan report for 192.168.50.6
>Host is up (0.12s latency).
>Nmap scan report for 192.168.50.8
>Host is up (0.12s latency).
>...
>Nmap done: 254 IP addresses (13 hosts up) scanned in 3.74 seconds
># =====================================
>```

Using nmap to perform a network sweep and then using grep to find live hosts
>``` shell
>kali@kali:~$ nmap -v -sn 192.168.50.1-253 -oG ping-sweep.txt
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 03:21 EST
>Initiating Ping Scan at 03:21
>...
>Read data files from: /usr/bin/../share/nmap
>Nmap done: 254 IP addresses (13 hosts up) scanned in 3.74 seconds
>...
># =====================================
>
>kali@kali:~$ grep Up ping-sweep.txt | cut -d " " -f 2
>
># ========== Expected Result ==========
>192.168.50.6
>192.168.50.8
>192.168.50.9
>...
># =====================================
>```

Using nmap to scan for web servers using port 80
>``` shell
>kali@kali:~$ nmap -p 80 192.168.50.1-253 -oG web-sweep.txt
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 03:50 EST
>Nmap scan report for 192.168.50.6
>Host is up (0.11s latency).
>
>PORT   STATE SERVICE
>80/tcp open  http
>
>Nmap scan report for 192.168.50.8
>Host is up (0.11s latency).
>
>PORT   STATE  SERVICE
>80/tcp closed http
>...
># =====================================
>
>kali@kali:~$ grep open web-sweep.txt | cut -d" " -f2
>
># ========== Expected Result ==========
>192.168.50.6
>192.168.50.20
>192.168.50.21
># =====================================
>```

Using nmap to perform a top twenty port scan, saving the output in greppable format
>``` shell
>kali@kali:~$ nmap -sT -A --top-ports=20 192.168.50.1-253 -oG top-port-sweep.txt
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 04:04 EST
>Nmap scan report for 192.168.50.6
>Host is up (0.12s latency).
>
>PORT     STATE  SERVICE       VERSION
>21/tcp   closed ftp
>22/tcp   open   ssh           OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
>| ssh-hostkey:
>|   3072 56:57:11:b5:dc:f1:13:d3:50:88:b8:ab:a9:83:e2:29 (RSA)
>|   256 4f:1d:f2:55:cb:40:e0:76:b4:36:90:19:a2:ba:f0:44 (ECDSA)
>|_  256 67:46:b3:97:26:a9:e3:a8:4d:eb:20:b3:9b:8d:7a:32 (ED25519)
>23/tcp   closed telnet
>25/tcp   closed smtp
>53/tcp   closed domain
>80/tcp   open   http          Apache httpd 2.4.41 ((Ubuntu))
>|_http-server-header: Apache/2.4.41 (Ubuntu)
>|_http-title: Under Construction
>110/tcp  closed pop3
>111/tcp  closed rpcbind
>...
># =====================================
>```
