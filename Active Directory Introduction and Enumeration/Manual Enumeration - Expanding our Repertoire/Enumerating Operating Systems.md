# Enumerating Operating Systems

Partial domain computer overview
>``` shell
>PS C:\Tools> Get-NetComputer
>
># ========== Expected Result ==========
>pwdlastset                    : 10/2/2022 10:19:40 PM
>logoncount                    : 319
>msds-generationid             : {89, 27, 90, 188...}
>serverreferencebl             : CN=DC1,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=corp,DC=com
>badpasswordtime               : 12/31/1600 4:00:00 PM
>distinguishedname             : CN=DC1,OU=Domain Controllers,DC=corp,DC=com
>objectclass                   : {top, person, organizationalPerson, user...}
>lastlogontimestamp            : 10/13/2022 11:37:06 AM
>name                          : DC1
>objectsid                     : S-1-5-21-1987370270-658905905-1781884369-1000
>samaccountname                : DC1$
>localpolicyflags              : 0
>codepage                      : 0
>samaccounttype                : MACHINE_ACCOUNT
>whenchanged                   : 10/13/2022 6:37:06 PM
>accountexpires                : NEVER
>countrycode                   : 0
>operatingsystem               : Windows Server 2022 Standard
>instancetype                  : 4
>msdfsr-computerreferencebl    : CN=DC1,CN=Topology,CN=Domain System Volume,CN=DFSR-GlobalSettings,CN=System,DC=corp,DC=com
>objectguid                    : 8db9e06d-068f-41bc-945d-221622bca952
>operatingsystemversion        : 10.0 (20348)
>lastlogoff                    : 12/31/1600 4:00:00 PM
>objectcategory                : CN=Computer,CN=Schema,CN=Configuration,DC=corp,DC=com
>dscorepropagationdata         : {9/2/2022 11:10:48 PM, 1/1/1601 12:00:01 AM}
>serviceprincipalname          : {TERMSRV/DC1, TERMSRV/DC1.corp.com, Dfsr-12F9A27C-BF97-4787-9364-D31B6C55EB04/DC1.corp.com, ldap/DC1.corp.com/ForestDnsZones.corp.com...}
>usncreated                    : 12293
>lastlogon                     : 10/18/2022 3:37:56 AM
>badpwdcount                   : 0
>cn                            : DC1
>useraccountcontrol            : SERVER_TRUST_ACCOUNT, TRUSTED_FOR_DELEGATION
>whencreated                   : 9/2/2022 11:10:48 PM
>primarygroupid                : 516
>iscriticalsystemobject        : True
>msds-supportedencryptiontypes : 28
>usnchanged                    : 178663
>ridsetreferences              : CN=RID Set,CN=DC1,OU=Domain Controllers,DC=corp,DC=com
>dnshostname                   : DC1.corp.com
># =====================================
>```

Displaying OS and hostname
>``` shell
>PS C:\Tools> Get-NetComputer | select operatingsystem,dnshostname
>
># ========== Expected Result ==========
>operatingsystem              dnshostname
>---------------              -----------
>Windows Server 2022 Standard DC1.corp.com
>Windows Server 2022 Standard web04.corp.com
>Windows Server 2022 Standard FILES04.corp.com
>Windows 11 Pro               client74.corp.com
>Windows 11 Pro               client75.corp.com
>Windows 10 Pro               CLIENT76.corp.com
># =====================================
>```

Lab 1 - Start VM Group 1 and log in to CLIENT75 as stephanie. Repeat the PowerView enumeration steps as outlined in this section. What is the DistinguishedName for the WEB04 machine?
>``` shell
>
>```
>

Lab 2 - Continue enumerating the operating systems in VM Group 1. What is the exact operating system version for FILES04? The answer is numbers in the format xx.x (xxxxx).
>``` shell
>
>```
>

Lab 3 - Start VM Group 2 and log in to CLIENT75 as stephanie. Use PowerView to enumerate the operating systems in the modified corp.com domain to obtain the flag.
>``` shell
>
>```
>
