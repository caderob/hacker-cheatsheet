# Active Directory - Enumeration Using Legacy Windows Tools

Connecting to the Windows 11 client using "xfreerdp"
>``` shell
>kali@kali:~$ xfreerdp /u:stephanie /d:corp.com /v:192.168.50.75
>```

Running "net user" to display users in the domain
>``` shell
>C:\Users\stephanie>net user /domain
>
># ========== Expected Result ==========
>The request will be processed at a domain controller for domain corp.com.
>
>User accounts for \\DC1.corp.com
>
>-------------------------------------------------------------------------------
>Administrator            dave                     Guest
>iis_service              jeff                     jeffadmin
>jen                      krbtgt                   pete
>stephanie
>The command completed successfully.
># =====================================
>```

Running "net user" against a specific user
>``` shell
>C:\Users\stephanie>net user jeffadmin /domain
>
># ========== Expected Result ==========
>The request will be processed at a domain controller for domain corp.com.
>
>User name                    jeffadmin
>Full Name
>Comment
>User's comment
>Country/region code          000 (System Default)
>Account active               Yes
>Account expires              Never
>
>Password last set            9/2/2022 4:26:48 PM
>Password expires             Never
>Password changeable          9/3/2022 4:26:48 PM
>Password required            Yes
>User may change password     Yes
>
>Workstations allowed         All
>Logon script
>User profile
>Home directory
>Last logon                   9/20/2022 1:36:09 AM
>
>Logon hours allowed          All
>
>Local Group Memberships      *Administrators
>Global Group memberships     *Domain Users         *Domain Admins
>The command completed successfully.
># =====================================
>```

Running "net group" to display groups in the domain
>``` shell
>C:\Users\stephanie>net group /domain
>
># ========== Expected Result ==========
>The request will be processed at a domain controller for domain corp.com.
>
>Group Accounts for \\DC1.corp.com
>
>-------------------------------------------------------------------------------
>*Cloneable Domain Controllers
>*Debug
>*Development Department
>*DnsUpdateProxy
>*Domain Admins
>*Domain Computers
>*Domain Controllers
>*Domain Guests
>*Domain Users
>*Enterprise Admins
>*Enterprise Key Admins
>*Enterprise Read-only Domain Controllers
>*Group Policy Creator Owners
>*Key Admins
>*Management Department
>*Protected Users
>*Read-only Domain Controllers
>*Sales Department
>*Schema Admins
>The command completed successfully.
># =====================================
>```

Running "net group" to display members in specific group
>``` shell
>PS C:\Tools> net group "Sales Department" /domain
>
># ========== Expected Result ==========
>The request will be processed at a domain controller for domain corp.com.
>
>Group name     Sales Department
>Comment
>
>Members
>
>-------------------------------------------------------------------------------
>pete                     stephanie
>The command completed successfully.
># =====================================
>```

Lab 1 - Which type of server acts as the core and hub of a domain hosted in Active Directory?
>``` shell
>
>```
>

Lab 2 - Start VM Group 1 and log in to CLIENT75 as stephanie. Use net.exe to enumerate the corp.com domain. Which user is a member of the Management Department group?
>``` shell
>
>```
>

Lab 3 - Start VM Group 2 and log in to CLIENT75 as stephanie. Use net.exe to enumerate the users and groups in the modified corp.com domain to obtain the flag.
>``` shell
>
>```
>
