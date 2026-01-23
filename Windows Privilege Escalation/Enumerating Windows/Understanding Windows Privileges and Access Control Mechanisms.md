# Understanding Windows Privileges and Access Control Mechanisms

SID representation (1)
>``` shell
>S-R-X-Y
>```

SID representation (2)
>``` shell
>S-1-5-21-1336799502-1441772794-948155058-1001
>```

List of Well known SIDs on local machines
>``` shell
>S-1-0-0                       Nobody        
>S-1-1-0	                     Everybody
>S-1-5-11                      Authenticated Users
>S-1-5-18                      Local System
>S-1-5-domainidentifier-500    Administrator
>```

Different Integrity Levels of PowerShell
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Understanding-Windows-Privileges-and-Access-Control-Mechanisms-1.png)

Lab 1 - What is the RID of the first standard user?
>1000

Lab 2 - Answer with true or false: An access token is generated when a user is created and is immutable.
>False
