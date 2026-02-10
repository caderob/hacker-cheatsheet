# MAILSRV1

Network Overview of provided Targets
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/MAILSRV1-1.png)

Basic work environment for this penetration test
>``` shell
>kali@kali:~$ mkdir beyond
>
>kali@kali:~$ cd beyond
>
>kali@kali:~/beyond$ mkdir mailsrv1
>
>kali@kali:~/beyond$ mkdir websrv1
>
>kali@kali:~/beyond$ touch creds.txt
>```

Nmap scan of MAILSRV1
>``` shell
>kali@kali:~/beyond$ sudo nmap -sC -sV -oN mailsrv1/nmap 192.168.50.242
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-09-29 08:53 EDT
>Nmap scan report for 192.168.50.242
>Host is up (0.11s latency).
>Not shown: 992 closed tcp ports (reset)
>PORT    STATE SERVICE       VERSION
>25/tcp  open  smtp          hMailServer smtpd
>| smtp-commands: MAILSRV1, SIZE 20480000, AUTH LOGIN, HELP
>|_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
>80/tcp  open  http          Microsoft IIS httpd 10.0
>|_http-title: IIS Windows Server
>| http-methods: 
>|_  Potentially risky methods: TRACE
>|_http-server-header: Microsoft-IIS/10.0
>110/tcp open  pop3          hMailServer pop3d
>|_pop3-capabilities: UIDL USER TOP
>135/tcp open  msrpc         Microsoft Windows RPC
>139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
>143/tcp open  imap          hMailServer imapd
>|_imap-capabilities: IMAP4 CHILDREN OK ACL IMAP4rev1 completed CAPABILITY NAMESPACE IDLE RIGHTS=texkA0001 SORT QUOTA
>445/tcp open  microsoft-ds?
>587/tcp open  smtp          hMailServer smtpd
>| smtp-commands: MAILSRV1, SIZE 20480000, AUTH LOGIN, HELP
>|_ 211 DATA HELO EHLO MAIL NOOP QUIT RCPT RSET SAML TURN VRFY
>Service Info: Host: MAILSRV1; OS: Windows; CPE: cpe:/o:microsoft:windows
>
>Host script results:
>| smb2-time: 
>|   date: 2022-09-29T12:54:00
>|_  start_date: N/A
>| smb2-security-mode: 
>|   3.1.1: 
>|_    Message signing enabled but not required
>|_clock-skew: 21s
>
>Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
>Nmap done: 1 IP address (1 host up) scanned in 37.95 seconds
># =====================================
>```

Vulnerabilities of hMailServer
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/MAILSRV1-2.png)

IIS Welcome Page on MAILSRV1
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/MAILSRV1-3.png)

Using gobuster to identify pages and files on MAILSRV1
>``` shell
>kali@kali:~/beyond$ gobuster dir -u http://192.168.50.242 -w /usr/share/wordlists/dirb/common.txt -o mailsrv1/gobuster -x txt,pdf,config 
>
># ========== Expected Result ==========
>===============================================================
>Gobuster v3.1.0
>by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
>===============================================================
>[+] Url:                     http://192.168.50.242
>[+] Method:                  GET
>[+] Threads:                 10
>[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
>[+] Negative Status codes:   404
>[+] User Agent:              gobuster/3.1.0
>[+] Extensions:              txt,pdf,config
>[+] Timeout:                 10s
>===============================================================
>2022/09/29 11:12:27 Starting gobuster in directory enumeration mode
>===============================================================
>
>                                
>===============================================================
>2022/09/29 11:16:00 Finished
>===============================================================
># =====================================
>```

Lab 1 - Start the VM group to follow along the guided penetration test throughout the Module. Once you have access to the domain controller, retrieve the NTLM hash of the domain administrator account BEYOND\Administrator and enter it as answer to this exercise. Please make sure you are dumping the NTLM hash of the domain administrator user with RID 500 by utilizing dcsync attack via mimikatz, not by extracting creds from SAM file. The hashes will be different.
>``` shell
>
>```
>
