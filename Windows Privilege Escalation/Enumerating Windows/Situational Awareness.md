# Situational Awareness

Information we should gather to obtain situational awareness
>``` shell
>- Username and hostname
>- Group memberships of the current user
>- Existing users and groups
>- Operating system, version and architecture
>- Network information
>- Installed applications
>- Running processes
>```

Connect to the bind shell and obtain username and hostname
>``` shell
>kali@kali:~$ nc 192.168.50.220 4444
>
># ========== Expected Result ==========
>Microsoft Windows [Version 10.0.22000.318]
>(c) Microsoft Corporation. All rights reserved.
># =====================================
>
>C:\Users\dave>whoami
>
># ========== Expected Result ==========
>whoami
>clientwk220\dave
># =====================================
>```

Group memberships of the user dave
>``` shell
>C:\Users\dave> whoami /groups
>
># ========== Expected Result ==========
>whoami /groups
>
>GROUP INFORMATION
>-----------------
>
>Group Name                             Type             SID                                            Attributes                                        
>====================================== ================ ============================================== ==================================================
>Everyone                             Well-known group S-1-1-0                                        Mandatory group, Enabled by default, Enabled group
>CLIENTWK220\helpdesk                 Alias            S-1-5-21-2309961351-4093026482-2223492918-1008 Mandatory group, Enabled by default, Enabled group
>BUILTIN\Remote Desktop Users         Alias            S-1-5-32-555                                   Mandatory group, Enabled by default, Enabled group
>BUILTIN\Users                        Alias            S-1-5-32-545                                   Mandatory group, Enabled by default, Enabled group
>NT AUTHORITY\BATCH                   Well-known group S-1-5-3                                        Mandatory group, Enabled by default, Enabled group
>CONSOLE LOGON                        Well-known group S-1-2-1                                        Mandatory group, Enabled by default, Enabled group
>NT AUTHORITY\Authenticated Users     Well-known group S-1-5-11                                       Mandatory group, Enabled by default, Enabled group
>... 
># =====================================
>```

Display local users on CLIENT220
>``` shell
>C:\Users\dave> powershell
>
># ========== Expected Result ==========
>powershell
>Windows PowerShell
>Copyright (C) Microsoft Corporation. All rights reserved.
>
>Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows
># =====================================
>
>PS C:\Users\dave> Get-LocalUser
>
># ========== Expected Result ==========
>Get-LocalUser
>
>Name               Enabled Description                                                                              
>----               ------- -----------                                                                              
>Administrator      False   Built-in account for administering the computer/domain
>BackupAdmin        True
>dave               True    dave 
>daveadmin          True 
>DefaultAccount     False   A user account managed by the system.
>Guest              False   Built-in account for guest access to the computer/domain
>offsec             True
>steve              True
>... 
># =====================================
>```

Display local groups on CLIENTWK220
>``` shell
>PS C:\Users\dave> Get-LocalGroup
>
># ========== Expected Result ==========
>Get-LocalGroup
>
>Name                                Description                                                                      
>----                                -----------                                                                     
>adminteam                  Members of this group are admins to all workstations on the second floor
>BackupUsers 
>helpdesk
>...
>Administrators                      Administrators have complete and unrestricted access to the computer/domain
>...
>Remote Desktop Users                Members in this group are granted the right to logon remotely
>...
># =====================================
>```

Display members of the group adminteam
>``` shell
>PS C:\Users\dave> Get-LocalGroupMember adminteam
>
># ========== Expected Result ==========
>Get-LocalGroupMember adminteam
>
>ObjectClass Name                PrincipalSource
>----------- ----                ---------------
>User        CLIENTWK220\daveadmin Local 
># =====================================
>
>PS C:\Users\dave> Get-LocalGroupMember Administrators
>
># ========== Expected Result ==========
>Get-LocalGroupMember Administrators
>
>ObjectClass Name                      PrincipalSource
>----------- ----                      ---------------
>User        CLIENTWK220\Administrator Local          
>User        CLIENTWK220\daveadmin     Local
>User        CLIENTWK220\backupadmin     Local  
>User        CLIENTWK220\offsec        Local
># =====================================
>```

Information about the operating system and architecture
>``` shell
>PS C:\Users\dave> systeminfo
>
># ========== Expected Result ==========
>systeminfo
>
>Host Name:                 CLIENTWK220
>OS Name:                   Microsoft Windows 11 Pro
>OS Version:                10.0.22621 N/A Build 22621
>...
>System Type:               x64-based PC
>...
># =====================================
>```

Information about the operating system and architecture
>``` shell
>PS C:\Users\dave> systeminfo
>
># ========== Expected Result ==========
>systeminfo
>
>Host Name:                 CLIENTWK220
>OS Name:                   Microsoft Windows 11 Pro
>OS Version:                10.0.22621 N/A Build 22621
>...
>System Type:               x64-based PC
>...
># =====================================
>```

Information about the network configuration
>``` shell
>PS C:\Users\dave> ipconfig /all
>
># ========== Expected Result ==========
>ipconfig /all
>
>Windows IP Configuration
>
>   Host Name . . . . . . . . . . . . : clientwk220
>   Primary Dns Suffix  . . . . . . . : 
>   Node Type . . . . . . . . . . . . : Hybrid
>   IP Routing Enabled. . . . . . . . : No
>   WINS Proxy Enabled. . . . . . . . : No
>
>Ethernet adapter Ethernet0:
>
>   Connection-specific DNS Suffix  . : 
>   Description . . . . . . . . . . . : vmxnet3 Ethernet Adapter
>   Physical Address. . . . . . . . . : 00-50-56-8A-80-16
>   DHCP Enabled. . . . . . . . . . . : No
>   Autoconfiguration Enabled . . . . : Yes
>   Link-local IPv6 Address . . . . . : fe80::cc7a:964e:1f98:babb%6(Preferred) 
>   IPv4 Address. . . . . . . . . . . : 192.168.50.220(Preferred) 
>   Subnet Mask . . . . . . . . . . . : 255.255.255.0
>   Default Gateway . . . . . . . . . : 192.168.50.254
>   DHCPv6 IAID . . . . . . . . . . . : 234901590
>   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-2A-3B-F7-25-00-50-56-8A-80-16
>   DNS Servers . . . . . . . . . . . : 8.8.8.8
>   NetBIOS over Tcpip. . . . . . . . : Enabled
># =====================================
>```

Information about the network configuration
>``` shell
>PS C:\Users\dave> ipconfig /all
>
># ========== Expected Result ==========
>ipconfig /all
>
>Windows IP Configuration
>
>   Host Name . . . . . . . . . . . . : clientwk220
>   Primary Dns Suffix  . . . . . . . : 
>   Node Type . . . . . . . . . . . . : Hybrid
>   IP Routing Enabled. . . . . . . . : No
>   WINS Proxy Enabled. . . . . . . . : No
>
>Ethernet adapter Ethernet0:
>
>   Connection-specific DNS Suffix  . : 
>   Description . . . . . . . . . . . : vmxnet3 Ethernet Adapter
>   Physical Address. . . . . . . . . : 00-50-56-8A-80-16
>   DHCP Enabled. . . . . . . . . . . : No
>   Autoconfiguration Enabled . . . . : Yes
>   Link-local IPv6 Address . . . . . : fe80::cc7a:964e:1f98:babb%6(Preferred) 
>   IPv4 Address. . . . . . . . . . . : 192.168.50.220(Preferred) 
>   Subnet Mask . . . . . . . . . . . : 255.255.255.0
>   Default Gateway . . . . . . . . . : 192.168.50.254
>   DHCPv6 IAID . . . . . . . . . . . : 234901590
>   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-2A-3B-F7-25-00-50-56-8A-80-16
>   DNS Servers . . . . . . . . . . . : 8.8.8.8
>   NetBIOS over Tcpip. . . . . . . . : Enabled
># =====================================
>```

Routing table on CLIENTWK220
>``` shell
>PS C:\Users\dave> route print
>
># ========== Expected Result ==========
>route print
>===========================================================================
>Interface List
>  4...00 50 56 95 01 6a ......vmxnet3 Ethernet Adapter
>  1...........................Software Loopback Interface 1
>===========================================================================
>
>IPv4 Route Table
>===========================================================================
>Active Routes:
>Network Destination        Netmask          Gateway       Interface  Metric
>          0.0.0.0          0.0.0.0   192.168.50.254   192.168.50.220    271
>        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
>        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
>  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
>     192.168.50.0    255.255.255.0         On-link    192.168.50.220    271
>   192.168.50.220  255.255.255.255         On-link    192.168.50.220    271
>   192.168.50.255  255.255.255.255         On-link    192.168.50.220    271
>        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
>        224.0.0.0        240.0.0.0         On-link    192.168.50.220    271
>  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
>  255.255.255.255  255.255.255.255         On-link    192.168.50.220    271
>===========================================================================
>Persistent Routes:
>  Network Address          Netmask  Gateway Address  Metric
>          0.0.0.0          0.0.0.0   192.168.50.254  Default 
>===========================================================================
>
>IPv6 Route Table
>===========================================================================
>Active Routes:
> If Metric Network Destination      Gateway
>  1    331 ::1/128                  On-link
>  4    271 fe80::/64                On-link
>  4    271 fe80::1b30:4f11:8789:866a/128
>                                    On-link
>  1    331 ff00::/8                 On-link
>  4    271 ff00::/8                 On-link
>===========================================================================
>Persistent Routes:
>  None
># =====================================
>```

Active network connections on CLIENTWK220
>``` shell
>PS C:\Users\dave> netstat -ano
>
># ========== Expected Result ==========
>netstat -ano
>
>Active Connections
>
>  Proto  Local Address          Foreign Address        State           PID
>  TCP    0.0.0.0:80             0.0.0.0:0              LISTENING       3340
>  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING       1016
>  TCP    0.0.0.0:443            0.0.0.0:0              LISTENING       3340
>  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING       4
>  TCP    0.0.0.0:3306           0.0.0.0:0              LISTENING       3508
>  TCP    0.0.0.0:3389           0.0.0.0:0              LISTENING       1148
>  TCP    192.168.50.220:139     0.0.0.0:0              LISTENING       4
>  TCP    192.168.50.220:3389    192.168.48.3:33770     ESTABLISHED     1148
>  TCP    192.168.50.220:4444    192.168.48.3:58386     ESTABLISHED     2064
>...
># =====================================
>```

Installed applications on CLIENTWK220
>``` shell
>PS C:\Users\dave> Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
>
># ========== Expected Result ==========
>Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname 
>
>displayname                                                       
>-----------                                                       
>                                                                  
>FileZilla 3.63.1                                                  
>KeePass Password Safe 2.51.1                                   
>Microsoft Edge                                                    
>Microsoft Edge Update                                             
>Microsoft Edge WebView2 Runtime                                   
>                                                                  
>Microsoft Visual C++ 2015-2019 Redistributable (x86) - 14.28.29913
>Microsoft Visual C++ 2019 X86 Additional Runtime - 14.28.29913    
>Microsoft Visual C++ 2019 X86 Minimum Runtime - 14.28.29913       
>Microsoft Visual C++ 2015-2019 Redistributable (x64) - 14.28.29913
># =====================================
>
>PS C:\Users\dave> Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
>
># ========== Expected Result ==========
>Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
>
>DisplayName                                                   
>-----------                                                   
>7-Zip 21.07 (x64)                                             
>                                                              
>                                                              
>XAMPP                                                         
>VMware Tools                                                  
>Microsoft Visual C++ 2019 X64 Additional Runtime - 14.28.29913
>Microsoft Update Health Tools                                 
>Microsoft Visual C++ 2019 X64 Minimum Runtime - 14.28.29913   
>Update for Windows 10 for x64-based Systems (KB5001716) 
># =====================================
>```

Installed applications on CLIENTWK220
>``` shell
>PS C:\Users\dave> Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
>
># ========== Expected Result ==========
>Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname 
>
>displayname                                                       
>-----------                                                       
>                                                                  
>FileZilla 3.63.1                                                  
>KeePass Password Safe 2.51.1                                   
>Microsoft Edge                                                    
>Microsoft Edge Update                                             
>Microsoft Edge WebView2 Runtime                                   
>                                                                  
>Microsoft Visual C++ 2015-2019 Redistributable (x86) - 14.28.29913
>Microsoft Visual C++ 2019 X86 Additional Runtime - 14.28.29913    
>Microsoft Visual C++ 2019 X86 Minimum Runtime - 14.28.29913       
>Microsoft Visual C++ 2015-2019 Redistributable (x64) - 14.28.29913
># =====================================
>
>PS C:\Users\dave> Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
>
># ========== Expected Result ==========
>Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
>
>DisplayName                                                   
>-----------                                                   
>7-Zip 21.07 (x64)                                             
>                                                              
>                                                              
>XAMPP                                                         
>VMware Tools                                                  
>Microsoft Visual C++ 2019 X64 Additional Runtime - 14.28.29913
>Microsoft Update Health Tools                                 
>Microsoft Visual C++ 2019 X64 Minimum Runtime - 14.28.29913   
>Update for Windows 10 for x64-based Systems (KB5001716) 
># =====================================
>```

Running processes on CLIENTWK220
>``` shell
>PS C:\Users\dave> Get-Process
>
># ========== Expected Result ==========
>Get-Process
>
>Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName                                                  
>-------  ------    -----      -----     ------     --  -- -----------                                                  
>     58      13      528       1088       0.00   2064   0 access                                                       
>...                                                  
>    369      32     9548      31320              2632   0 filezilla                                                    
>...                                         
>    188      29     9596      19716              3340   0 httpd                                                        
>    486      49    16528      23060              4316   0 httpd                                                        
>...                                                   
>    205      17   210736      29228              3508   0 mysqld                                                       
>...                                     
>    982      32    83696      13780       0.59   2836   0 powershell                                                   
>    587      28    65628      73752              9756   0 powershell                                                   
>...
># =====================================
>```

Lab 1 - Check the users of the local group Remote Management Users on CLIENTWK220 (VM #1). Enter a user which is in this group apart from steve.
>``` shell
>
>```
>

Lab 2 - Enumerate the installed applications on CLIENTWK220 (VM #1) and find the flag.
>``` shell
>
>```
>

Lab 3 - We'll now use an additional machine, CLIENTWK221 (VM #2), to practice what we learned in this section. Access the machine via RDP as user mac with the password IAmTheGOATSysAdmin!. Identify another member of the local Administrators group apart from offsec and Administrator.
>``` shell
>
>```
>

Lab 4 - Enumerate the currently running processes on CLIENTWK221 (VM #2). Find a non-standard process and locate the flag in the directory of the corresponding binary file.
>``` shell
>
>```
>
