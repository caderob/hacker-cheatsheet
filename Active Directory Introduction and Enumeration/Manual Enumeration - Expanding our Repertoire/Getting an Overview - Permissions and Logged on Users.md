# Getting an Overview - Permissions and Logged on Users

Scanning domain to find local administrative privileges for our user
>``` shell
>PS C:\Tools> Find-LocalAdminAccess
>
># ========== Expected Result ==========
>client74.corp.com
># =====================================
>```

Checking logged on users with Get-NetSession
>``` shell
>PS C:\Tools> Get-NetSession -ComputerName files04
>
>PS C:\Tools> Get-NetSession -ComputerName web04
>```

Adding verbosity to our Get-NetSession command
>``` shell
>PS C:\Tools> Get-NetSession -ComputerName files04 -Verbose
>
># ========== Expected Result ==========
>VERBOSE: [Get-NetSession] Error: Access is denied
># =====================================
>
>PS C:\Tools> Get-NetSession -ComputerName web04 -Verbose
>
># ========== Expected Result ==========
>VERBOSE: [Get-NetSession] Error: Access is denied
># =====================================
>```

Running Get-NetSession on CLIENT74
>``` shell
>PS C:\Tools> Get-NetSession -ComputerName client74
>
># ========== Expected Result ==========
>CName        : \\192.168.50.75
>UserName     : stephanie
>Time         : 8
>IdleTime     : 0
>ComputerName : client74
># =====================================
>```

Displaying permissions on the DefaultSecurity registry hive
>``` shell
>PS C:\Tools> Get-Acl -Path HKLM:SYSTEM\CurrentControlSet\Services\LanmanServer\DefaultSecurity\ | fl
>
># ========== Expected Result ==========
>Path   : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\DefaultSecurity\
>Owner  : NT AUTHORITY\SYSTEM
>Group  : NT AUTHORITY\SYSTEM
>Access : BUILTIN\Users Allow  ReadKey
>         BUILTIN\Administrators Allow  FullControl
>         NT AUTHORITY\SYSTEM Allow  FullControl
>         CREATOR OWNER Allow  FullControl
>         APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES Allow  ReadKey
>         S-1-15-3-1024-1065365936-1281604716-3511738428-1654721687-432734479-3232135806-4053264122-3456934681 Allow  ReadKey
># =====================================
>```

Querying operating system and version
>``` shell
>PS C:\Tools> Get-NetComputer | select dnshostname,operatingsystem,operatingsystemversion
>
># ========== Expected Result ==========
>dnshostname       operatingsystem              operatingsystemversion
>-----------       ---------------              ----------------------
>DC1.corp.com      Windows Server 2022 Standard 10.0 (20348)
>web04.corp.com    Windows Server 2022 Standard 10.0 (20348)
>FILES04.corp.com  Windows Server 2022 Standard 10.0 (20348)
>client74.corp.com Windows 11 Pro               10.0 (22000)
>client75.corp.com Windows 11 Pro               10.0 (22000)
>CLIENT76.corp.com Windows 10 Pro               10.0 (16299)
># =====================================
>```

Using PsLoggedOn to see user logons at Files04
>``` shell
>PS C:\Tools\PSTools> .\PsLoggedon.exe \\files04
>
># ========== Expected Result ==========
>PsLoggedon v1.35 - See who's logged on
>Copyright (C) 2000-2016 Mark Russinovich
>Sysinternals - www.sysinternals.com
>
>Users logged on locally:
>     <unknown time>             CORP\jeff
>Unable to query resource logons
># =====================================
>```

Using PsLoggedOn to see user logons at Web04
>``` shell
>PS C:\Tools\PSTools> .\PsLoggedon.exe \\web04
>
># ========== Expected Result ==========
>PsLoggedon v1.35 - See who's logged on
>Copyright (C) 2000-2016 Mark Russinovich
>Sysinternals - www.sysinternals.com
>
>No one is logged on locally.
>Unable to query resource logons
># =====================================
>```

Using PsLoggedOn to see user logons at CLIENT74
>``` shell
>PS C:\Tools\PSTools> .\PsLoggedon.exe \\client74
>
># ========== Expected Result ==========
>PsLoggedon v1.35 - See who's logged on
>Copyright (C) 2000-2016 Mark Russinovich
>Sysinternals - www.sysinternals.com
>
>Users logged on locally:
>     <unknown time>             CORP\jeffadmin
>
>Users logged on via resource shares:
>     10/5/2022 1:33:32 AM       CORP\stephanie
># =====================================
>```

Lab 1 - What registry key does NetSessionEnum rely on to discover logged on sessions? Submit the name of the registry key as the answer, not the path.
>``` shell
>
>```
>

Lab 2 - Start VM Group 1 and log in to CLIENT75 as stephanie. Repeat the enumeration steps outlined in this section to find the logged on sessions. Which service must be enabled on the remote machine to make it possible for PsLoggedOn to enumerate sessions?
>``` shell
>
>```
>

Lab 3 - Start VM Group 2 and log in to CLIENT75 as stephanie. Find out which new machine stephanie has administrative privileges on, then log in to that machine and obtain the flag from the Administrator Desktop.
>``` shell
>
>```
>
