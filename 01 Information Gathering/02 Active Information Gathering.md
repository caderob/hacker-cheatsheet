# Active Information Gathering

## DNS Enumeration

Using host to find the A host record for www.megacorpone.com
>``` shell
>kali@kali:~$ host www.megacorpone.com
>
># ========== Expected Result ==========
>www.megacorpone.com has address 149.56.244.87
># =====================================
>```

Using host to find the MX records for megacorpone.com
>``` shell
>kali@kali:~$ host -t mx megacorpone.com
>
># ========== Expected Result ==========
>megacorpone.com mail is handled by 10 fb.mail.gandi.net.
>megacorpone.com mail is handled by 20 spool.mail.gandi.net.
>megacorpone.com mail is handled by 50 mail.megacorpone.com.
>megacorpone.com mail is handled by 60 mail2.megacorpone.com.
># =====================================
>```

Using host to find the TXT records for megacorpone.com
>``` shell
>kali@kali:~$ host -t txt megacorpone.com
>
># ========== Expected Result ==========
>megacorpone.com descriptive text "Try Harder"
>megacorpone.com descriptive text "google-site-verification=U7B_b0HNeBtY4qYGQZNsEYXfCJ32hMNV3GtC0wWq5pA"
># =====================================
>```

Using host to search for a valid host
>``` shell
>kali@kali:~$ host www.megacorpone.com
>
># ========== Expected Result ==========
>www.megacorpone.com has address 149.56.244.87 
># =====================================
>```

Using host to search for an invalid host
>``` shell
>kali@kali:~$ host idontexist.megacorpone.com
>
># ========== Expected Result ==========
>Host idontexist.megacorpone.com not found: 3(NXDOMAIN)
># =====================================
>```

## TCP/UDP Port Scanning Theory

## Port Scanning with Nmap

## SMB Enumeration

## SMTP Enumeration

## SNMP Enumeration
