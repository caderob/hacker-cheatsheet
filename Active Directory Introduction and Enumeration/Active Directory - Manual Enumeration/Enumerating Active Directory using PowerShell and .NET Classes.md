# Enumerating Active Directory using PowerShell and .NET Classes

LDAP path format
>``` shell
>LDAP://HostName[:PortNumber][/DistinguishedName]
>```

Example of a Distinguished Name
>``` shell
>CN=Stephanie,CN=Users,DC=corp,DC=com
>```

Domain class from System.DirectoryServices.ActiveDirectory namespace
>``` shell
>PS C:\Users\stephanie> [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
>
># ========== Expected Result ==========
>Forest                  : corp.com
>DomainControllers       : {DC1.corp.com}
>Children                : {}
>DomainMode              : Unknown
>DomainModeLevel         : 7
>Parent                  :
>PdcRoleOwner        : DC1.corp.com
>RidRoleOwner            : DC1.corp.com
>InfrastructureRoleOwner : DC1.corp.com
>Name                  	: corp.com
># =====================================
>```

Storing domain object in our first variable
>``` shell
># Store the domain object in the $domainObj variable
>$domainObj = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
>
># Print the variable
>$domainObj
>```

>``` shell
>PS C:\Users\stephanie> powershell -ep bypass
>
># ========== Expected Result ==========
>Windows PowerShell
>Copyright (C) Microsoft Corporation. All rights reserved.
>
>Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows
># =====================================
>```

Output displaying information stored in our first variable
>``` shell
>PS C:\Users\stephanie> .\enumeration.ps1
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

Adding the $PDC variable to our script and extracting PdcRoleOwner name to it
>``` shell
># Store the domain object in the $domainObj variable
>$domainObj = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
>
># Store the PdcRoleOwner name to the $PDC variable
>$PDC = $domainObj.PdcRoleOwner.Name
>
># Print the $PDC variable
>$PDC
>```

Printing the $PDC variable
>``` shell
>PS C:\Users\stephanie> .\enumeration.ps1
>
># ========== Expected Result ==========
>DC1.corp.com
># =====================================
>```

Using ADSI to obtain the DN for the domain
>``` shell
>PS C:\Users\stephanie> ([adsi]'').distinguishedName
>
># ========== Expected Result ==========
>DC=corp,DC=com
># =====================================
>```

Creating a new variable holding the DN for the domain
>``` shell
># Store the domain object in the $domainObj variable
>$domainObj = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
>
># Store the PdcRoleOwner name to the $PDC variable
>$PDC = $domainObj.PdcRoleOwner.Name
>
># Store the Distinguished Name variable into the $DN variable
>$DN = ([adsi]'').distinguishedName
>
># Print the $DN variable
>$DN
>```

Using our script to print the DN of the domain
>``` shell
>PS C:\Users\stephanie> .\enumeration.ps1
>
># ========== Expected Result ==========
>DC=corp,DC=com
># =====================================
>```

Script which will create the full LDAP path required for enumeration
>``` shell
>$PDC = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().PdcRoleOwner.Name
>$DN = ([adsi]'').distinguishedName 
>$LDAP = "LDAP://$PDC/$DN"
>$LDAP
>```

Script output showing the full LDAP path
>``` shell
>PS C:\Users\stephanie> .\enumeration.ps1
>
># ========== Expected Result ==========
>LDAP://DC1.corp.com/DC=corp,DC=com
># =====================================
>```

Lab 1 - Start VM Group 1 and repeat the steps outlined in this section to build the script. Use the script to dynamically obtain the LDAP path for the corp.com domain. Which property in the domain object shows the primary domain controller for the domain?
>``` shell
>
>```
>

Lab 2 - Which set of COM interfaces gives us an LDAP provider we can use for communication with Active Directory?
>``` shell
>
>```
>
