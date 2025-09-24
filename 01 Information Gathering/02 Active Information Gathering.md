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

Using Bash to brute force forward DNS name lookups
>``` shell
>kali@kali:~$ for ip in $(cat list.txt); do host $ip.megacorpone.com; done
>
># ========== Expected Result ==========
>www.megacorpone.com has address 149.56.244.87
>Host ftp.megacorpone.com not found: 3(NXDOMAIN)
>mail.megacorpone.com has address 51.222.169.212
>Host owa.megacorpone.com not found: 3(NXDOMAIN)
>Host proxy.megacorpone.com not found: 3(NXDOMAIN)
>router.megacorpone.com has address 51.222.169.214
># =====================================
>```

Using Bash to brute force reverse DNS names
>``` shell
>kali@kali:~$ for ip in $(seq 200 254); do host 51.222.169.$ip; done | grep -v "not found"
>
># ========== Expected Result ==========
>...
>208.169.222.51.in-addr.arpa domain name pointer admin.megacorpone.com.
>209.169.222.51.in-addr.arpa domain name pointer beta.megacorpone.com.
>210.169.222.51.in-addr.arpa domain name pointer fs1.megacorpone.com.
>211.169.222.51.in-addr.arpa domain name pointer intranet.megacorpone.com.
>212.169.222.51.in-addr.arpa domain name pointer mail.megacorpone.com.
>213.169.222.51.in-addr.arpa domain name pointer mail2.megacorpone.com.
>214.169.222.51.in-addr.arpa domain name pointer router.megacorpone.com.
>215.169.222.51.in-addr.arpa domain name pointer siem.megacorpone.com.
>216.169.222.51.in-addr.arpa domain name pointer snmp.megacorpone.com.
>217.169.222.51.in-addr.arpa domain name pointer syslog.megacorpone.com.
>218.169.222.51.in-addr.arpa domain name pointer support.megacorpone.com.
>219.169.222.51.in-addr.arpa domain name pointer test.megacorpone.com.
>220.169.222.51.in-addr.arpa domain name pointer vpn.megacorpone.com.
>...
># =====================================
>```

## TCP/UDP Port Scanning Theory

## Port Scanning with Nmap

## SMB Enumeration

## SMTP Enumeration

## SNMP Enumeration
