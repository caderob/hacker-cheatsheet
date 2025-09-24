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

Using nmap for OS fingerprinting
>``` shell
>kali@kali:~$ sudo nmap -O 192.168.50.14 --osscan-guess
>
># ========== Expected Result ==========
>...
>Running (JUST GUESSING): Microsoft Windows 2008|2012|2016|7|Vista (88%)
>OS CPE: cpe:/o:microsoft:windows_server_2008::sp1 cpe:/o:microsoft:windows_server_2008:r2 cpe:/o:microsoft:windows_server_2012:r2 cpe:/o:microsoft:windows_server_2016 cpe:/o:microsoft:windows_7 cpe:/o:microsoft:windows_vista::sp1:home_premium
>Aggressive OS guesses: Microsoft Windows Server 2008 SP1 or Windows Server 2008 R2 (88%), Microsoft Windows Server 2012 or Windows Server 2012 R2 (88%), Microsoft Windows Server 2012 R2 (88%), Microsoft Windows Server 2012 (87%), Microsoft Windows Server 2016 (87%), Microsoft Windows 7 (86%), Microsoft Windows Vista Home Premium SP1 (85%), Microsoft Windows 7 Professional (85%)
>No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
>...
># =====================================
>```

Using nmap for banner grabbing and/or service enumeration
>``` shell
>kali@kali:~$ nmap -sT -A 192.168.50.14
>
># ========== Expected Result ==========
>Nmap scan report for 192.168.50.14
>Host is up (0.12s latency).
>Not shown: 996 closed tcp ports (conn-refused)
>PORT    STATE SERVICE       VERSION
>21/tcp  open  ftp?
>| fingerprint-strings:
>|   DNSStatusRequestTCP, DNSVersionBindReqTCP, GenericLines, NULL, RPCCheck, SSLSessionReq, TLSSessionReq, TerminalServerCookie:
>|     220-FileZilla Server 1.2.0
>|     Please visit https://filezilla-project.org/
>|   GetRequest:
>|     220-FileZilla Server 1.2.0
>|     Please visit https://filezilla-project.org/
>|     What are you trying to do? Go away.
>|   HTTPOptions, RTSPRequest:
>|     220-FileZilla Server 1.2.0
>|     Please visit https://filezilla-project.org/
>|     Wrong command.
>|   Help:
>|     220-FileZilla Server 1.2.0
>|     Please visit https://filezilla-project.org/
>|     214-The following commands are recognized.
>|     USER TYPE SYST SIZE RNTO RNFR RMD REST QUIT
>|     HELP XMKD MLST MKD EPSV XCWD NOOP AUTH OPTS DELE
>|     CDUP APPE STOR ALLO RETR PWD FEAT CLNT MFMT
>|     MODE XRMD PROT ADAT ABOR XPWD MDTM LIST MLSD PBSZ
>|     NLST EPRT PASS STRU PASV STAT PORT
>|_    Help ok.
>| ftp-syst:
>|_  SYST: UNIX emulated by FileZilla.
>| ssl-cert: Subject: commonName=filezilla-server self signed certificate
>| Not valid before: 2022-01-06T15:37:24
>|_Not valid after:  2023-01-07T15:42:24
>|_ssl-date: TLS randomness does not represent time
>135/tcp open  msrpc         Microsoft Windows RPC
>139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
>445/tcp open  microsoft-ds?
>Nmap done: 1 IP address (1 host up) scanned in 55.67 seconds
># =====================================
>```

Using nmap's scripting engine (NSE) for OS fingerprinting
>``` shell
>kali@kali:~$ nmap --script http-headers 192.168.50.6
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 13:53 EST
>Nmap scan report for 192.168.50.6
>Host is up (0.14s latency).
>Not shown: 998 closed tcp ports (conn-refused)
>PORT   STATE SERVICE
>22/tcp open  ssh
>80/tcp open  http
>| http-headers:
>|   Date: Thu, 10 Mar 2022 18:53:29 GMT
>|   Server: Apache/2.4.41 (Ubuntu)
>|   Last-Modified: Thu, 10 Mar 2022 18:51:54 GMT
>|   ETag: "d1-5d9e1b5371420"
>|   Accept-Ranges: bytes
>|   Content-Length: 209
>|   Vary: Accept-Encoding
>|   Connection: close
>|   Content-Type: text/html
>|
>|_  (Request type: HEAD)
>
>Nmap done: 1 IP address (1 host up) scanned in 5.11 seconds
># =====================================
>```

Using the --script-help option to view more information about a script
>``` shell
>kali@kali:~$ nmap --script-help http-headers
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 13:54 EST
>
>http-headers
>Categories: discovery safe
>https://nmap.org/nsedoc/scripts/http-headers.html
>  Performs a HEAD request for the root folder ("/") of a web server and displays the HTTP headers returned.
>...
># =====================================
>```

Port scanning SMB via PowerShell
>``` shell
>PS C:\Users\student> Test-NetConnection -Port 445 192.168.50.151
>
># ========== Expected Result ==========
>ComputerName     : 192.168.50.151
>RemoteAddress    : 192.168.50.151
>RemotePort       : 445
>InterfaceAlias   : Ethernet0
>SourceAddress    : 192.168.50.152
>TcpTestSucceeded : True
># =====================================
>```

Automating the PowerShell portscanning
>``` shell
>PS C:\Users\student> 1..1024 | % {echo ((New-Object Net.Sockets.TcpClient).Connect("192.168.50.151", $_)) "TCP port $_ is open"} 2>$null
>
># ========== Expected Result ==========
>TCP port 88 is open
>...
># =====================================
>```

Lab 1 - Which host has port 25 open?
>``` shell
>kali@kali:~$ sudo nmap -sS 192.168.135.1-254 -oG syn-scan.txt
>
># ========== Expected Result ==========
>Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-24 11:07 CDT
>Nmap scan report for 192.168.170.6
>Host is up (0.035s latency).
>Not shown: 998 closed tcp ports (reset)
>PORT   STATE SERVICE
>22/tcp open  ssh
>80/tcp open  http
>...
># =====================================
>
>kali@kali:~$ grep open syn-scan.txt | cut -d" " -f2 | sort -u
>
># ========== Expected Result ==========
>192.168.170.11
>192.168.170.12
>192.168.170.13
>192.168.170.14
>192.168.170.149
>192.168.170.15
>192.168.170.151
>192.168.170.152
>192.168.170.20
>192.168.170.21
>192.168.170.22
>192.168.170.251
>192.168.170.254
>192.168.170.6
>192.168.170.8
>192.168.170.9
># =====================================
>
>kali@kali:~$ grep "25/open" syn-scan.txt
>
># ========== Expected Result ==========
>Host: 192.168.170.8 ()  Ports: 22/open/tcp//ssh///, 25/open/tcp//smtp///        Ignored State: closed (998)
># =====================================
>```
>192.168.170.8
