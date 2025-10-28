# SMTP Enumeration

Using nc to validate SMTP users
>``` shell
>kali@kali:~$ nc -nv 192.168.50.8 25
>
># ========== Expected Result ==========
>(UNKNOWN) [192.168.50.8] 25 (smtp) open
>220 mail ESMTP Postfix (Ubuntu)
>VRFY root
>252 2.0.0 root
>VRFY idontexist
>550 5.1.1 <idontexist>: Recipient address rejected: User unknown in local recipient table
>^C
># =====================================
>```

Using Python to script the SMTP user enumeration
>``` python
>#!/usr/bin/python
>
>import socket
>import sys
>
>if len(sys.argv) != 3:
>        print("Usage: vrfy.py <username> <target_ip>")
>        sys.exit(0)
>
># Create a Socket
>s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
>
># Connect to the Server
>ip = sys.argv[2]
>connect = s.connect((ip,25))
>
># Receive the banner
>banner = s.recv(1024)
>
>print(banner)
>
># VRFY a user
>user = (sys.argv[1]).encode()
>s.send(b'VRFY ' + user + b'\r\n')
>result = s.recv(1024)
>
>print(result)
>
># Close the socket
>s.close()
>```

Running the Python script to perform SMTP user enumeration
>``` shell
>kali@kali:~$ python3 smtp.py root 192.168.50.8
>
># ========== Expected Result ==========
>b'220 mail ESMTP Postfix (Ubuntu)\r\n'
>b'252 2.0.0 root\r\n'
># =====================================
>
>kali@kali:~$ python3 smtp.py johndoe 192.168.50.8
>
># ========== Expected Result ==========
>b'220 mail ESMTP Postfix (Ubuntu)\r\n'
>b'550 5.1.1 <johndoe>: Recipient address rejected: User unknown in local recipient table\r\n'
># =====================================
>```

Port scanning SMB via PowerShell
>``` shell
>PS C:\Users\student> Test-NetConnection -Port 25 192.168.50.8
>
># ========== Expected Result ==========
>ComputerName     : 192.168.50.8
>RemoteAddress    : 192.168.50.8
>RemotePort       : 25
>InterfaceAlias   : Ethernet0
>SourceAddress    : 192.168.50.152
>TcpTestSucceeded : True
># =====================================
>```

Installing the Telnet client
>``` shell
>PS C:\Windows\system32> dism /online /Enable-Feature /FeatureName:TelnetClient
>```
>Note: installing Telnet requires administrative privileges, which could present challenges if we are running as a low-privilege user. However, we could grab the Telnet binary located on another development machine of ours at c:\windows\system32\telnet.exe and transfer it to the Windows machine we are testing from.

Interacting with the SMTP service via Telnet on Windows
>``` shell
>C:\Windows\system32>telnet 192.168.50.8 25
>
># ========== Expected Result ==========
>220 mail ESMTP Postfix (Ubuntu)
>VRFY goofy
>550 5.1.1 <goofy>: Recipient address rejected: User unknown in local recipient table
>VRFY root
>252 2.0.0 root
># =====================================
>```

Lab 1 - Power on the Walk Through Exercises VM Group 1 and search your target network range to identify any systems that respond to SMTP. Once found, open a connection to port 25 via Netcat and run VRFY command against the root user. What response code does the SMTP server send as a response?
>``` shell
># Scan for hosts with SMTP (port 25) open
>kali@kali:~$ sudo nmap -v -p 25 --open -oG smtp-scan.txt 192.168.149.1-254
>
># ========== Expected Result ==========
>Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-30 11:02 CDT
>Initiating Ping Scan at 11:02
>Scanning 254 hosts [4 ports/host]
>Completed Ping Scan at 11:02, 3.46s elapsed (254 total hosts)
>Initiating Parallel DNS resolution of 17 hosts. at 11:02
>Completed Parallel DNS resolution of 17 hosts. at 11:02, 0.00s elapsed
>Initiating SYN Stealth Scan at 11:02
>Scanning 17 hosts [1 port/host]
>Discovered open port 25/tcp on 192.168.149.8
>Completed SYN Stealth Scan at 11:02, 0.43s elapsed (17 total ports)
>Nmap scan report for 192.168.149.8
>Host is up (0.036s latency).
>
>PORT   STATE SERVICE
>25/tcp open  smtp
>
>Read data files from: /usr/share/nmap
>Nmap done: 254 IP addresses (17 hosts up) scanned in 4.00 seconds
>           Raw packets sent: 1934 (73.432KB) | Rcvd: 46 (1.928KB)
># =====================================
>
># Extract IPs
>kali@kali:~$ grep "25/open" smtp-scan.txt | cut -d" " -f2
>
># ========== Expected Result ==========
>192.168.149.8
># =====================================
>
># Open a connection to port 25 via Netcat
>kali@kali:~$ nc 192.168.149.8 25
>
># ========== Expected Result ==========
>220 mail ESMTP Postfix (Ubuntu)
>VRFY root
>252 2.0.0 root
>^C
># =====================================
>```
>252
