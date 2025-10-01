# Active Information Gathering

## SNMP Enumeration

Using nc to validate SMTP users
>``` shell
>kali@kali:~$ nc -nv 192.168.50.8 25
>
># ========== Expected Result ==========
>(UNKNOWN) [192.168.50.8] 25 (smtp) open
>220 mail ESMTP Postfix (Ubuntu)
>VRFY root
>252 2.0.0 root
>VRFY idontexist
>550 5.1.1 <idontexist>: Recipient address rejected: User unknown in local recipient table
>^C
># =====================================
