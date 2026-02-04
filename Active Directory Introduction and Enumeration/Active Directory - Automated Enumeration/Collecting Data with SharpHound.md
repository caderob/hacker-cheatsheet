# Collecting Data with SharpHound

Importing the SharpHound script to memory
>``` shell
>PS C:\Users\stephanie> cd .\Downloads\
>
>PS C:\Users\stephanie\Downloads> powershell -ep bypass
>
># ========== Expected Result ==========
>Windows PowerShell
>Copyright (C) Microsoft Corporation. All rights reserved.
>
>Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows
># =====================================
>
>PS C:\Users\stephanie\Downloads> Import-Module .\Sharphound.ps1
>```

Checking the SharpHound options
>``` shell
>PS C:\Users\stephanie\Downloads> Get-Help Invoke-BloodHound
>
># ========== Expected Result ==========
>NAME
>    Invoke-BloodHound
>
>SYNOPSIS
>    Runs the BloodHound C# Ingestor using reflection. The assembly is stored in this file.
>
>
>SYNTAX
>    Invoke-BloodHound [-CollectionMethods <String[]>] [-Domain <String>] [-SearchForest] [-Stealth] [-LdapFilter
>    <String>] [-DistinguishedName <String>] [-ComputerFile <String>] [-OutputDirectory <String>] [-OutputPrefix
>    <String>] [-CacheName <String>] [-MemCache] [-RebuildCache] [-RandomFilenames] [-ZipFilename <String>] [-NoZip]
>    [-ZipPassword <String>] [-TrackComputerCalls] [-PrettyPrint] [-LdapUsername <String>] [-LdapPassword <String>]
>    [-DomainController <String>] [-LdapPort <Int32>] [-SecureLdap] [-DisableCertVerification] [-DisableSigning]
>    [-SkipPortCheck] [-PortCheckTimeout <Int32>] [-SkipPasswordCheck] [-ExcludeDCs] [-Throttle <Int32>] [-Jitter
>    <Int32>] [-Threads <Int32>] [-SkipRegistryLoggedOn] [-OverrideUsername <String>] [-RealDNSName <String>]
>    [-CollectAllProperties] [-Loop] [-LoopDuration <String>] [-LoopInterval <String>] [-StatusInterval <Int32>]
>    [-Verbosity <Int32>] [-Help] [-Version] [<CommonParameters>]
>
>
>DESCRIPTION
>    Using reflection and assembly.load, load the compiled BloodHound C# ingestor into memory
>    and run it without touching disk. Parameters are converted to the equivalent CLI arguments
>    for the SharpHound executable and passed in via reflection. The appropriate function
>    calls are made in order to ensure that assembly dependencies are loaded properly.
>
>
>RELATED LINKS
>
>REMARKS
>    To see the examples, type: "get-help Invoke-BloodHound -examples".
>    For more information, type: "get-help Invoke-BloodHound -detailed".
>    For technical information, type: "get-help Invoke-BloodHound -full".
># =====================================
>```

Running SharpHound to collect domain data
>``` shell
>PS C:\Users\stephanie\Downloads> Invoke-BloodHound -CollectionMethod All -OutputDirectory C:\Users\stephanie\Desktop\ -OutputPrefix "corp audit"
>```

SharpHound output
>``` shell
>2024-08-10T20:16:00.6554069-07:00|INFORMATION|This version of SharpHound is compatible with the 5.0.0 Release of BloodHound
>2024-08-10T20:16:00.7960323-07:00|INFORMATION|Resolved Collection Methods: Group, LocalAdmin, GPOLocalGroup, Session, LoggedOn, Trusts, ACL, Container, RDP, ObjectProps, DCOM, SPNTargets, PSRemote, UserRights, CARegistry, DCRegistry, CertServices
>2024-08-10T20:16:00.8429091-07:00|INFORMATION|Initializing SharpHound at 8:16 PM on 8/10/2024
>2024-08-10T20:16:00.8741609-07:00|INFORMATION|Resolved current domain to corp.com
>2024-08-10T20:16:00.9835316-07:00|INFORMATION|Flags: Group, LocalAdmin, GPOLocalGroup, Session, LoggedOn, Trusts, ACL, Container, RDP, ObjectProps, DCOM, SPNTargets, PSRemote, UserRights, CARegistry, DCRegistry, CertServices
>2024-08-10T20:16:01.0616591-07:00|INFORMATION|Beginning LDAP search for corp.com
>2024-08-10T20:16:01.1241627-07:00|INFORMATION|Beginning LDAP search for corp.com Configuration NC
>2024-08-10T20:16:01.1397817-07:00|INFORMATION|Producer has finished, closing LDAP channel
>2024-08-10T20:16:01.1554037-07:00|INFORMATION|LDAP channel closed, waiting for consumers
>2024-08-10T20:16:01.7022783-07:00|INFORMATION|Consumers finished, closing output channel
>Closing writers
>2024-08-10T20:16:01.7179066-07:00|INFORMATION|Output channel closed, waiting for output task to complete
>2024-08-10T20:16:01.8272851-07:00|INFORMATION|Status: 309 objects finished (+309 Infinity)/s -- Using 118 MB RAM
>2024-08-10T20:16:01.8272851-07:00|INFORMATION|Enumeration finished in 00:00:00.7702863
>2024-08-10T20:16:01.8897888-07:00|INFORMATION|Saving cache with stats: 19 ID to type mappings.
> 2 name to SID mappings.
> 6 machine sid mappings.
> 4 sid to domain mappings.
> 0 global catalog mappings.
>2024-08-10T20:16:01.9054062-07:00|INFORMATION|SharpHound Enumeration Completed at 8:16 PM on 8/10/2024! Happy Graphing!
>```

SharpHound generated files
>``` shell
>PS C:\Users\stephanie\Downloads> ls C:\Users\stephanie\Desktop\
>
># ========== Expected Result ==========
>    Directory: C:\Users\stephanie\Desktop
>
>
>Mode                 LastWriteTime         Length Name
>----                 -------------         ------ ----
>-a----         8/10/2024   8:16 PM          26255 corp audit_20240810201601_BloodHound.zip
>-a----         8/10/2024   8:16 PM           2110 MTk2MmZkNjItY2IyNC00MWMzLTk5YzMtM2E1ZDcwYThkMzRl.bin
># =====================================
>```

Lab 1 - Start VM Group 1 and log in to CLIENT75 as stephanie. Gather the domain data with SharpHound as outlined in this section. Which function can we use with SharpHound to see changes happening in the domain over a longer period of time? Submit the answer as one word without hyphens.
>``` shell
>
>```
>

Lab 2 - Which syntax in SharpHound allows us to set a password on the resulting .zip file?
>``` shell
>
>```
>
