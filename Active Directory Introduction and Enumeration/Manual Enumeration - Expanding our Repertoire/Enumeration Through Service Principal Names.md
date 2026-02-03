# Enumeration Through Service Principal Names

Listing SPN linked to a certain user account
>``` shell
>c:\Tools>setspn -L iis_service
>
># ========== Expected Result ==========
>Registered ServicePrincipalNames for CN=iis_service,CN=Users,DC=corp,DC=com:
>        HTTP/web04.corp.com
>        HTTP/web04
>        HTTP/web04.corp.com:80
># =====================================
>```

Listing the SPN accounts in the domain
>``` shell
>PS C:\Tools> Get-NetUser -SPN | select samaccountname,serviceprincipalname
>
># ========== Expected Result ==========
>samaccountname serviceprincipalname
>-------------- --------------------
>krbtgt         kadmin/changepw
>iis_service    {HTTP/web04.corp.com, HTTP/web04, HTTP/web04.corp.com:80}
># =====================================
>```

Resolving the web04.corp.com name
>``` shell
>PS C:\Tools\> nslookup.exe web04.corp.com
>
># ========== Expected Result ==========
>Server:  UnKnown
>Address:  192.168.50.70
>
>Name:    web04.corp.com
>Address:  192.168.50.72
># =====================================
>```

Web04 Login
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Enumeration-Through-Service-Principal-Names-1.png)

Lab 1 - Start VM Group 1 and log in to CLIENT75 as stephanie. Repeat the enumeration steps outlined in this section to enumerate the Service Account. What is the name of the unique service identifier that is used to associate to a specific service in Active Directory?
>``` shell
>
>```
>
