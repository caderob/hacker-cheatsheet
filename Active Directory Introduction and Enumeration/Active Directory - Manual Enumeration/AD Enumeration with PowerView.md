# AD Enumeration with PowerView

Importing PowerView to memory
>``` shell
>PS C:\Tools> Import-Module .\PowerView.ps1
>```

Obtaining domain information
>``` shell
>PS C:\Tools> Get-NetDomain
>
># ========== Expected Result ==========
>Forest                  : corp.com
>DomainControllers       : {DC1.corp.com}
>Children                : {}
>DomainMode              : Unknown
>DomainModeLevel         : 7
>Parent                  :
>PdcRoleOwner            : DC1.corp.com
>RidRoleOwner            : DC1.corp.com
>InfrastructureRoleOwner : DC1.corp.com
>Name                    : corp.com
># =====================================
>```

Querying users in the domain
>``` shell
>PS C:\Tools> Get-NetUser
>
># ========== Expected Result ==========
>logoncount             : 113
>iscriticalsystemobject : True
>description            : Built-in account for administering the computer/domain
>distinguishedname      : CN=Administrator,CN=Users,DC=corp,DC=com
>objectclass            : {top, person, organizationalPerson, user}
>lastlogontimestamp     : 9/13/2022 1:03:47 AM
>name                   : Administrator
>objectsid              : S-1-5-21-1987370270-658905905-1781884369-500
>samaccountname         : Administrator
>admincount             : 1
>codepage               : 0
>samaccounttype         : USER_OBJECT
>accountexpires         : NEVER
>cn                     : Administrator
>whenchanged            : 9/13/2022 8:03:47 AM
>instancetype           : 4
>usncreated             : 8196
>objectguid             : e5591000-080d-44c4-89c8-b06574a14d85
>lastlogoff             : 12/31/1600 4:00:00 PM
>objectcategory         : CN=Person,CN=Schema,CN=Configuration,DC=corp,DC=com
>dscorepropagationdata  : {9/2/2022 11:25:58 PM, 9/2/2022 11:25:58 PM, 9/2/2022 11:10:49 PM, 1/1/1601 6:12:16 PM}
>memberof               : {CN=Group Policy Creator Owners,CN=Users,DC=corp,DC=com, CN=Domain Admins,CN=Users,DC=corp,DC=com, CN=Enterprise
>                         Admins,CN=Users,DC=corp,DC=com, CN=Schema Admins,CN=Users,DC=corp,DC=com...}
>lastlogon              : 9/14/2022 2:37:15 AM
>...
># =====================================
>```

Querying users displaying pwdlastset and lastlogon
>``` shell
>PS C:\Tools> Get-NetUser | select cn,pwdlastset,lastlogon
>
># ========== Expected Result ==========
>cn            pwdlastset            lastlogon
>--            ----------            ---------
>Administrator 8/16/2022 5:27:22 PM  9/14/2022 2:37:15 AM
>Guest         12/31/1600 4:00:00 PM 12/31/1600 4:00:00 PM
>krbtgt        9/2/2022 4:10:48 PM   12/31/1600 4:00:00 PM
>dave          9/7/2022 9:54:57 AM   9/14/2022 2:57:28 AM
>stephanie     9/2/2022 4:23:38 PM   12/31/1600 4:00:00 PM
>jeff          9/2/2022 4:27:20 PM   9/14/2022 2:54:55 AM
>jeffadmin     9/2/2022 4:26:48 PM   9/14/2022 2:26:37 AM
>iis_service   9/7/2022 5:38:43 AM   9/14/2022 2:35:55 AM
>pete          9/6/2022 12:41:54 PM  9/13/2022 8:37:09 AM
>jen           9/6/2022 12:43:01 PM  9/13/2022 8:36:55 AM
># =====================================
>```

Querying groups in the domain using PowerView
>``` shell
>PS C:\Tools> Get-NetGroup | select cn
>
># ========== Expected Result ==========
>cn
>--
>...
>Key Admins
>Enterprise Key Admins
>DnsAdmins
>DnsUpdateProxy
>Sales Department
>Management Department
>Development Department
>Debug
># =====================================
>```

Enumerating the "Sales Department" group
>``` shell
>PS C:\Tools> Get-NetGroup "Sales Department" | select member
>
># ========== Expected Result ==========
>member
>------
>{CN=Development Department,DC=corp,DC=com, CN=pete,CN=Users,DC=corp,DC=com, CN=stephanie,CN=Users,DC=corp,DC=com}
># =====================================
>```

Lab 1 - Start VM Group 1 and log in to CLIENT75 as stephanie. Import the PowerView script to memory and repeat the enumeration steps outlined in this section. Which command can we use with PowerView to list the domain groups?
>``` shell
>
>```
>

Lab 2 - Start VM Group 2 and log in to CLIENT75 as stephanie. Use PowerView to enumerate the modified corp.com domain. Which new user is a part of the Domain Admins group?
>``` shell
>
>```
>

Lab 3 - Continue enumerating the corp.com domain in VM Group 2. Enumerate which Office the user fred is working in to obtain the flag.
>``` shell
>
>```
>
