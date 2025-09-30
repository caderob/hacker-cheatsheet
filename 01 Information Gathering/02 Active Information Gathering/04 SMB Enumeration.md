# Active Information Gathering

## SMB Enumeration

Using nmap to scan for the NetBIOS service
>``` shell
>kali@kali:~$ nmap -v -p 139,445 -oG smb.txt 192.168.50.1-254
>
>kali@kali:~$ cat smb.txt
>
># ========== Expected Result ==========
># Nmap 7.92 scan initiated Thu Mar 17 06:03:12 2022 as: nmap -v -p 139,445 -oG smb.txt 192.168.50.1-254
># Ports scanned: TCP(2;139,445) UDP(0;) SCTP(0;) PROTOCOLS(0;)
>Host: 192.168.50.1 ()	Status: Down
>...
>Host: 192.168.50.21 ()	Status: Up
>Host: 192.168.50.21 ()	Ports: 139/closed/tcp//netbios-ssn///, 445/closed/tcp//microsoft-ds///
>...
>Host: 192.168.50.217 ()	Status: Up
>Host: 192.168.50.217 ()	Ports: 139/closed/tcp//netbios-ssn///, 445/closed/tcp//microsoft-ds///
># Nmap done at Thu Mar 17 06:03:18 2022 -- 254 IP addresses (15 hosts up) scanned in 6.17 seconds
># =====================================
>```

Using nbtscan to collect additional NetBIOS information
>``` shell
>kali@kali:~$ sudo nbtscan -r 192.168.50.0/24
>
># ========== Expected Result ==========
>Doing NBT name scan for addresses from 192.168.50.0/24
>
>IP address       NetBIOS Name     Server    User             MAC address
>------------------------------------------------------------------------------
>192.168.50.124   SAMBA            <server>  SAMBA            00:00:00:00:00:00
>192.168.50.134   SAMBAWEB         <server>  SAMBAWEB         00:00:00:00:00:00
>...
># =====================================
>```



Lab 1 - Which host has port 25 open?
>``` shell
># Using nmap to perform a network sweep
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
># Grep to find live hosts
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
># Grep to find live hosts with port 25 open
>kali@kali:~$ grep "25/open" syn-scan.txt
>
># ========== Expected Result ==========
>Host: 192.168.170.8 ()  Ports: 22/open/tcp//ssh///, 25/open/tcp//smtp///        Ignored State: closed (998)
># =====================================
>```
>192.168.170.8
