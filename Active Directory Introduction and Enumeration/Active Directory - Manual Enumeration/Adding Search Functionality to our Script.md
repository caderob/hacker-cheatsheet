# Adding Search Functionality to our Script

Directory and DirectorySearcher to our script
>``` shell
>$PDC = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().PdcRoleOwner.Name
>$DN = ([adsi]'').distinguishedName 
>$LDAP = "LDAP://$PDC/$DN"
>
>$direntry = New-Object System.DirectoryServices.DirectoryEntry($LDAP)
>
>$dirsearcher = New-Object System.DirectoryServices.DirectorySearcher($direntry)
>$dirsearcher.FindAll()
>```

Using our script to search AD
>``` shell
>PS C:\Users\stephanie> .\enumeration.ps1
>
># ========== Expected Result ==========
>Path
>----
>LDAP://DC1.corp.com/DC=corp,DC=com
>LDAP://DC1.corp.com/CN=Users,DC=corp,DC=com
>LDAP://DC1.corp.com/CN=Computers,DC=corp,DC=com
>LDAP://DC1.corp.com/OU=Domain Controllers,DC=corp,DC=com
>LDAP://DC1.corp.com/CN=System,DC=corp,DC=com
>LDAP://DC1.corp.com/CN=LostAndFound,DC=corp,DC=com
>LDAP://DC1.corp.com/CN=Infrastructure,DC=corp,DC=com
>LDAP://DC1.corp.com/CN=ForeignSecurityPrincipals,DC=corp,DC=com
>LDAP://DC1.corp.com/CN=Program Data,DC=corp,DC=com
>LDAP://DC1.corp.com/CN=Microsoft,CN=Program Data,DC=corp,DC=com
>LDAP://DC1.corp.com/CN=NTDS Quotas,DC=corp,DC=com
>LDAP://DC1.corp.com/CN=Managed Service Accounts,DC=corp,DC=com
>LDAP://DC1.corp.com/CN=Keys,DC=corp,DC=com
>LDAP://DC1.corp.com/CN=WinsockServices,CN=System,DC=corp,DC=com
>LDAP://DC1.corp.com/CN=RpcServices,CN=System,DC=corp,DC=com
>LDAP://DC1.corp.com/CN=FileLinks,CN=System,DC=corp,DC=com
>LDAP://DC1.corp.com/CN=VolumeTable,CN=FileLinks,CN=System,DC=corp,DC=com
>LDAP://DC1.corp.com/CN=ObjectMoveTable,CN=FileLinks,CN=System,DC=corp,DC=com
>...
># =====================================
>```

Using samAccountType attribute to filter normal user accounts
>``` shell
>$PDC = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().PdcRoleOwner.Name
>$DN = ([adsi]'').distinguishedName 
>$LDAP = "LDAP://$PDC/$DN"
>
>$direntry = New-Object System.DirectoryServices.DirectoryEntry($LDAP)
>
>$dirsearcher = New-Object System.DirectoryServices.DirectorySearcher($direntry)
>$dirsearcher.filter="samAccountType=805306368"
>$dirsearcher.FindAll()
>```

Receiving all users in the domain filtering on samAccountType
>``` shell
>PS C:\Users\stephanie> .\enumeration.ps1
>
># ========== Expected Result ==========
>Path                                                         Properties
>----                                                         ----------
>LDAP://DC1.corp.com/CN=Administrator,CN=Users,DC=corp,DC=com {logoncount, codepage, objectcategory, description...}
>LDAP://DC1.corp.com/CN=Guest,CN=Users,DC=corp,DC=com         {logoncount, codepage, objectcategory, description...}
>LDAP://DC1.corp.com/CN=krbtgt,CN=Users,DC=corp,DC=com        {logoncount, codepage, objectcategory, description...}
>LDAP://DC1.corp.com/CN=dave,CN=Users,DC=corp,DC=com          {logoncount, codepage, objectcategory, usnchanged...}
>LDAP://DC1.corp.com/CN=stephanie,CN=Users,DC=corp,DC=com     {logoncount, codepage, objectcategory, dscorepropagatio...
>LDAP://DC1.corp.com/CN=jeff,CN=Users,DC=corp,DC=com          {logoncount, codepage, objectcategory, dscorepropagatio...
>LDAP://DC1.corp.com/CN=jeffadmin,CN=Users,DC=corp,DC=com     {logoncount, codepage, objectcategory, dscorepropagatio...
>LDAP://DC1.corp.com/CN=iis_service,CN=Users,DC=corp,DC=com   {logoncount, codepage, objectcategory, dscorepropagatio...
>LDAP://DC1.corp.com/CN=pete,CN=Users,DC=corp,DC=com          {logoncount, codepage, objectcategory, dscorepropagatio...
>LDAP://DC1.corp.com/CN=jen,CN=Users,DC=corp,DC=com           {logoncount, codepage, objectcategory, dscorepropagatio
># =====================================
>```

Adding a nested loop which will print each property on its own line
>``` shell
>$domainObj = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
>$PDC = $domainObj.PdcRoleOwner.Name
>$DN = ([adsi]'').distinguishedName 
>$LDAP = "LDAP://$PDC/$DN"
>
>$direntry = New-Object System.DirectoryServices.DirectoryEntry($LDAP)
>
>$dirsearcher = New-Object System.DirectoryServices.DirectorySearcher($direntry)
>$dirsearcher.filter="samAccountType=805306368"
>$result = $dirsearcher.FindAll()
>
>Foreach($obj in $result)
>{
>    Foreach($prop in $obj.Properties)
>    {
>        $prop
>    }
>
>    Write-Host "-------------------------------"
>}
>```

Running script, printing each attribute for "jeffadmin"
>``` shell
>PS C:\Users\stephanie> .\enumeration.ps1
>
># ========== Expected Result ==========
>...
>logoncount                     {173}
>codepage                       {0}
>objectcategory                 {CN=Person,CN=Schema,CN=Configuration,DC=corp,DC=com}
>dscorepropagationdata          {9/3/2022 6:25:58 AM, 9/2/2022 11:26:49 PM, 1/1/1601 12:00:00 AM}
>usnchanged                     {52775}
>instancetype                   {4}
>name                           {jeffadmin}
>badpasswordtime                {133086594569025897}
>pwdlastset                     {133066348088894042}
>objectclass                    {top, person, organizationalPerson, user}
>badpwdcount                    {0}
>samaccounttype                 {805306368}
>lastlogontimestamp             {133080434621989766}
>usncreated                     {12821}
>objectguid                     {14 171 173 158 0 247 44 76 161 53 112 209 139 172 33 163}
>memberof                       {CN=Domain Admins,CN=Users,DC=corp,DC=com, CN=Administrators,CN=Builtin,DC=corp,DC=com}
>whencreated                    {9/2/2022 11:26:48 PM}
>adspath                        {LDAP://DC1.corp.com/CN=jeffadmin,CN=Users,DC=corp,DC=com}
>useraccountcontrol             {66048}
>cn                             {jeffadmin}
>countrycode                    {0}
>primarygroupid                 {513}
>whenchanged                    {9/19/2022 6:44:22 AM}
>lockouttime                    {0}
>lastlogon                      {133088312288347545}
>distinguishedname              {CN=jeffadmin,CN=Users,DC=corp,DC=com}
>admincount                     {1}
>samaccountname                 {jeffadmin}
>objectsid                      {1 5 0 0 0 0 0 5 21 0 0 0 30 221 116 118 49 27 70 39 209 101 53 106 82 4 0 0}
>lastlogoff                     {0}
>accountexpires                 {9223372036854775807}
>...
># =====================================
>```

Adding the name property to the filter and only print the "memberof" attribute in the nested loop
>``` shell
>$dirsearcher = New-Object System.DirectoryServices.DirectorySearcher($direntry)
>$dirsearcher.filter="name=jeffadmin"
>$result = $dirsearcher.FindAll()
>
>Foreach($obj in $result)
>{
>    Foreach($prop in $obj.Properties)
>    {
>        $prop.memberof
>    }
>
>    Write-Host "-------------------------------"
>}
>```

Running script to only show jeffadmin and which groups he is a member of
>``` shell
>PS C:\Users\stephanie> .\enumeration.ps1
>
># ========== Expected Result ==========
>CN=Domain Admins,CN=Users,DC=corp,DC=com
>CN=Administrators,CN=Builtin,DC=corp,DC=com
># =====================================
>```

A function that accepts user input
>``` shell
>function LDAPSearch {
>    param (
>        [string]$LDAPQuery
>    )
>
>    $PDC = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().PdcRoleOwner.Name
>    $DistinguishedName = ([adsi]'').distinguishedName
>
>    $DirectoryEntry = New-Object System.DirectoryServices.DirectoryEntry("LDAP://$PDC/$DistinguishedName")
>
>    $DirectorySearcher = New-Object System.DirectoryServices.DirectorySearcher($DirectoryEntry, $LDAPQuery)
>
>    return $DirectorySearcher.FindAll()
>
>}
>```

Importing our function to memory
>``` shell
>PS C:\Users\stephanie> Import-Module .\function.ps1
>```

Performing a user search using the new function
>``` shell
>PS C:\Users\stephanie> LDAPSearch -LDAPQuery "(samAccountType=805306368)"
>
># ========== Expected Result ==========
>Path                                                         Properties
>----                                                         ----------
>LDAP://DC1.corp.com/CN=Administrator,CN=Users,DC=corp,DC=com {logoncount, codepage, objectcategory, description...}
>LDAP://DC1.corp.com/CN=Guest,CN=Users,DC=corp,DC=com         {logoncount, codepage, objectcategory, description...}
>LDAP://DC1.corp.com/CN=krbtgt,CN=Users,DC=corp,DC=com        {logoncount, codepage, objectcategory, description...}
>LDAP://DC1.corp.com/CN=dave,CN=Users,DC=corp,DC=com          {logoncount, codepage, objectcategory, usnchanged...}
>LDAP://DC1.corp.com/CN=stephanie,CN=Users,DC=corp,DC=com     {logoncount, codepage, objectcategory, dscorepropagatio...
>LDAP://DC1.corp.com/CN=jeff,CN=Users,DC=corp,DC=com          {logoncount, codepage, objectcategory, dscorepropagatio...
>LDAP://DC1.corp.com/CN=jeffadmin,CN=Users,DC=corp,DC=com     {logoncount, codepage, objectcategory, dscorepropagatio...
>LDAP://DC1.corp.com/CN=iis_service,CN=Users,DC=corp,DC=com   {logoncount, codepage, objectcategory, dscorepropagatio...
>LDAP://DC1.corp.com/CN=pete,CN=Users,DC=corp,DC=com          {logoncount, codepage, objectcategory, dscorepropagatio...
>LDAP://DC1.corp.com/CN=jen,CN=Users,DC=corp,DC=com           {logoncount, codepage, objectcategory, dscorepropagatio
># =====================================
>```

Searching all possible groups in AD
>``` shell
>PS C:\Users\stephanie> LDAPSearch -LDAPQuery "(objectclass=group)"
>
># ========== Expected Result ==========
>...                                                                                 ----------
>LDAP://DC1.corp.com/CN=Read-only Domain Controllers,CN=Users,DC=corp,DC=com            {usnchanged, distinguishedname, grouptype, whencreated...}
>LDAP://DC1.corp.com/CN=Enterprise Read-only Domain Controllers,CN=Users,DC=corp,DC=com {iscriticalsystemobject, usnchanged, distinguishedname, grouptype...}
>LDAP://DC1.corp.com/CN=Cloneable Domain Controllers,CN=Users,DC=corp,DC=com            {iscriticalsystemobject, usnchanged, distinguishedname, grouptype...}
>LDAP://DC1.corp.com/CN=Protected Users,CN=Users,DC=corp,DC=com                         {iscriticalsystemobject, usnchanged, distinguishedname, grouptype...}
>LDAP://DC1.corp.com/CN=Key Admins,CN=Users,DC=corp,DC=com                              {iscriticalsystemobject, usnchanged, distinguishedname, grouptype...}
>LDAP://DC1.corp.com/CN=Enterprise Key Admins,CN=Users,DC=corp,DC=com                   {iscriticalsystemobject, usnchanged, distinguishedname, grouptype...}
>LDAP://DC1.corp.com/CN=DnsAdmins,CN=Users,DC=corp,DC=com                               {usnchanged, distinguishedname, grouptype, whencreated...}
>LDAP://DC1.corp.com/CN=DnsUpdateProxy,CN=Users,DC=corp,DC=com                          {usnchanged, distinguishedname, grouptype, whencreated...}
>LDAP://DC1.corp.com/CN=Sales Department,DC=corp,DC=com                                 {usnchanged, distinguishedname, grouptype, whencreated...}
>LDAP://DC1.corp.com/CN=Management Department,DC=corp,DC=com                            {usnchanged, distinguishedname, grouptype, whencreated...}
>LDAP://DC1.corp.com/CN=Development Department,DC=corp,DC=com                           {usnchanged, distinguishedname, grouptype, whencreated...}
>LDAP://DC1.corp.com/CN=Debug,CN=Users,DC=corp,DC=com                                   {usnchanged, distinguishedname, grouptype, whencreated...}
># =====================================
>```

Using "foreach" to iterate through the objects in $group variable
>``` shell
>PS C:\Users\stephanie\Desktop> foreach ($group in $(LDAPSearch -LDAPQuery "(objectCategory=group)")) {
>$group.properties | select {$_.cn}, {$_.member}
>}
>```

Partial output from our previous search
>``` shell
>...
>Sales Department              {CN=Development Department,DC=corp,DC=com, CN=pete,CN=Users,DC=corp,DC=com, CN=stephanie,CN=Users,DC=corp,DC=com}
>Management Department         CN=jen,CN=Users,DC=corp,DC=com
>Development Department        {CN=Management Department,DC=corp,DC=com, CN=pete,CN=Users,DC=corp,DC=com, CN=dave,CN=Users,DC=corp,DC=com}
>...
>```

Adding the search to our variable called $sales
>``` shell
>PS C:\Users\stephanie> $sales = LDAPSearch -LDAPQuery "(&(objectCategory=group)(cn=Sales Department))"
>```

Printing the member attribute on the Sales Department group object
>``` shell
>PS C:\Users\stephanie\Desktop> $sales.properties.member
>
># ========== Expected Result ==========
>CN=Development Department,DC=corp,DC=com
>CN=pete,CN=Users,DC=corp,DC=com
>CN=stephanie,CN=Users,DC=corp,DC=com
>PS C:\Users\stephanie\Desktop>
># =====================================
>```

Printing the member attribute on the Development Department group object
>``` shell
>PS C:\Users\stephanie> $group = LDAPSearch -LDAPQuery "(&(objectCategory=group)(cn=Development Department*))"
>
>PS C:\Users\stephanie> $group.properties.member
>
># ========== Expected Result ==========
>CN=Management Department,DC=corp,DC=com
>CN=pete,CN=Users,DC=corp,DC=com
>CN=dave,CN=Users,DC=corp,DC=com
># =====================================
>```

Printing the member attribute on the Management Department group object
>``` shell
>PS C:\Users\stephanie\Desktop> $group = LDAPSearch -LDAPQuery "(&(objectCategory=group)(cn=Management Department*))"
>
>PS C:\Users\stephanie\Desktop> $group.properties.member
>
># ========== Expected Result ==========
>CN=jen,CN=Users,DC=corp,DC=com
># =====================================
>```

Lab 1 - Start VM Group 1 and log in to CLIENT75 as stephanie. Follow the steps outlined in this section to add search functionality to the script. Encapsulate the script functionality into a function and repeat the enumeration process. Which .NET class makes the search against Active Directory?
>``` shell
>
>```
>

Lab 2 - Start VM Group 2 and log in to CLIENT75 as stephanie. Use the newly developed PowerShell script to enumerate the domain groups, starting with Service Personnel. Unravel the nested groups, then enumerate the attributes for the last direct user member of the nested groups to obtain the flag.
>``` shell
>
>```
>
