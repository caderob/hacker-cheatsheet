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

Finding various nmap SMB NSE scripts
>``` shell
>kali@kali:~$ ls -1 /usr/share/nmap/scripts/smb*
>
># ========== Expected Result ==========
>/usr/share/nmap/scripts/smb2-capabilities.nse
>/usr/share/nmap/scripts/smb2-security-mode.nse
>/usr/share/nmap/scripts/smb2-time.nse
>/usr/share/nmap/scripts/smb2-vuln-uptime.nse
>/usr/share/nmap/scripts/smb-brute.nse
>/usr/share/nmap/scripts/smb-double-pulsar-backdoor.nse
>/usr/share/nmap/scripts/smb-enum-domains.nse
>/usr/share/nmap/scripts/smb-enum-groups.nse
>/usr/share/nmap/scripts/smb-enum-processes.nse
>/usr/share/nmap/scripts/smb-enum-sessions.nse
>/usr/share/nmap/scripts/smb-enum-shares.nse
>/usr/share/nmap/scripts/smb-enum-users.nse
>/usr/share/nmap/scripts/smb-os-discovery.nse
>...
># =====================================
>```

Using the nmap scripting engine to perform OS discovery
>``` shell
>kali@kali:~$ nmap -v -p 139,445 --script smb-os-discovery 192.168.50.152
>
># ========== Expected Result ==========
>...
>PORT    STATE SERVICE      REASON
>139/tcp open  netbios-ssn  syn-ack
>445/tcp open  microsoft-ds syn-ack
>
>Host script results:
>| smb-os-discovery:
>|   OS: Windows 10 Pro 22000 (Windows 10 Pro 6.3)
>|   OS CPE: cpe:/o:microsoft:windows_10::-
>|   Computer name: client01
>|   NetBIOS computer name: CLIENT01\x00
>|   Domain name: megacorptwo.com
>|   Forest name: megacorptwo.com
>|   FQDN: client01.megacorptwo.com
>|_  System time: 2022-03-17T11:54:20-07:00
>...
># =====================================
>```

Running 'net view' to list remote shares
>``` shell
>C:\Users\student>net view \\dc01 /all
>
># ========== Expected Result ==========
>Shared resources at \\dc01
>
>Share name  Type  Used as  Comment
>
>-------------------------------------------------------------------------------
>ADMIN$      Disk           Remote Admin
>C$          Disk           Default share
>IPC$        IPC            Remote IPC
>NETLOGON    Disk           Logon server share
>SYSVOL      Disk           Logon server share
>The command completed successfully.
># =====================================
>```

Lab 1 - How many hosts have port 445 open?
>``` shell
># Perform SMB scan on port 445
>kali@kali:~$ sudo nmap -v -p 445 -oG smb-445-scan.txt 192.168.149.1-254
>
># ========== Expected Result ==========
>Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-30 09:51 CDT
>Initiating Ping Scan at 09:51
>Scanning 254 hosts [4 ports/host]
>Completed Ping Scan at 09:51, 3.34s elapsed (254 total hosts)
>Initiating Parallel DNS resolution of 17 hosts. at 09:51
>Completed Parallel DNS resolution of 17 hosts. at 09:51, 0.00s elapsed
>Nmap scan report for 192.168.149.1 [host down]
>Nmap scan report for 192.168.149.2 [host down]
>...
>Nmap scan report for 192.168.149.254
>Host is up (0.038s latency).
>
>PORT    STATE  SERVICE
>445/tcp closed microsoft-ds
>
>Read data files from: /usr/share/nmap
>Nmap done: 254 IP addresses (17 hosts up) scanned in 3.92 seconds
>           Raw packets sent: 1933 (73.388KB) | Rcvd: 45 (1.924KB)
># =====================================
>
># Count hosts with the port 445 open 
>kali@kali:~$ grep "445/open" smb-445-scan.txt | cut -d" " -f2 | sort -u | wc -l
>
># ========== Expected Result ==========
>10
># =====================================
>```
>10

Lab 2 - List remote shares from the domain controller
>``` shell
># RDP to Windows client host
>kali@kali:~$ xfreerdp3 /u:student /p:lab /v:192.168.149.152
>
># Running 'net view' to list remote shares
>C:\Users\student>net view \\dc01 /all
>
># ========== Expected Result ==========
>Shared resources at \\dc01
>
>
>
>Share name  Type  Used as  Comment
>
>-------------------------------------------------------------------------------
>ADMIN$      Disk           Remote Admin
>C$          Disk           Default share
>IPC$        IPC            Remote IPC
>NETLOGON    Disk           Logon server share
>SYSVOL      Disk           Logon server share
>The command completed successfully.
># =====================================
>```
>ADMIN$,C$,IPC$
