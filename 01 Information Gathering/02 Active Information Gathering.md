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

Using dnsrecon to perform a standard scan
>``` shell
>kali@kali:~$ dnsrecon -d megacorpone.com -t std
>
># ========== Expected Result ==========
>[*] std: Performing General Enumeration against: megacorpone.com...
>[-] DNSSEC is not configured for megacorpone.com
>[*] 	 SOA ns1.megacorpone.com 51.79.37.18
>[*] 	 NS ns1.megacorpone.com 51.79.37.18
>[*] 	 NS ns3.megacorpone.com 66.70.207.180
>[*] 	 NS ns2.megacorpone.com 51.222.39.63
>[*] 	 MX mail.megacorpone.com 51.222.169.212
>[*] 	 MX spool.mail.gandi.net 217.70.178.1
>[*] 	 MX fb.mail.gandi.net 217.70.178.217
>[*] 	 MX fb.mail.gandi.net 217.70.178.216
>[*] 	 MX fb.mail.gandi.net 217.70.178.215
>[*] 	 MX mail2.megacorpone.com 51.222.169.213
>[*] 	 TXT megacorpone.com Try Harder
>[*] 	 TXT megacorpone.com google-site-verification=U7B_b0HNeBtY4qYGQZNsEYXfCJ32hMNV3GtC0wWq5pA
>[*] Enumerating SRV Records
>[+] 0 Records Found
># =====================================
>```

Brute forcing hostnames using dnsrecon
>``` shell
>kali@kali:~$ dnsrecon -d megacorpone.com -D ~/list.txt -t brt
>
># ========== Expected Result ==========
>[*] Using the dictionary file: /home/kali/list.txt (provided by user)
>[*] brt: Performing host and subdomain brute force against megacorpone.com...
>[+] 	 A www.megacorpone.com 149.56.244.87
>[+] 	 A mail.megacorpone.com 51.222.169.212
>[+] 	 A router.megacorpone.com 51.222.169.214
>[+] 3 Records Found
># =====================================
>```

Using dnsenum to automate DNS enumeration
>``` shell
>kali@kali:~$ dnsenum megacorpone.com
>
># ========== Expected Result ==========
>...
>dnsenum VERSION:1.2.6
>
>-----   megacorpone.com   -----
>
>...
>
>Brute forcing with /usr/share/dnsenum/dns.txt:
>_______________________________________________
>
>admin.megacorpone.com.                   5        IN    A        51.222.169.208
>beta.megacorpone.com.                    5        IN    A        51.222.169.209
>fs1.megacorpone.com.                     5        IN    A        51.222.169.210
>intranet.megacorpone.com.                5        IN    A        51.222.169.211
>mail.megacorpone.com.                    5        IN    A        51.222.169.212
>mail2.megacorpone.com.                   5        IN    A        51.222.169.213
>ns1.megacorpone.com.                     5        IN    A        51.79.37.18
>ns2.megacorpone.com.                     5        IN    A        51.222.39.63
>ns3.megacorpone.com.                     5        IN    A        66.70.207.180
>router.megacorpone.com.                  5        IN    A        51.222.169.214
>siem.megacorpone.com.                    5        IN    A        51.222.169.215
>snmp.megacorpone.com.                    5        IN    A        51.222.169.216
>syslog.megacorpone.com.                  5        IN    A        51.222.169.217
>test.megacorpone.com.                    5        IN    A        51.222.169.219
>vpn.megacorpone.com.                     5        IN    A        51.222.169.220
>www.megacorpone.com.                     5        IN    A        149.56.244.87
>www2.megacorpone.com.                    5        IN    A        149.56.244.87
>
>
>megacorpone.com class C netranges:
>___________________________________
>
> 51.79.37.0/24
> 51.222.39.0/24
> 51.222.169.0/24
> 66.70.207.0/24
> 149.56.244.0/24
>
>
>Performing reverse lookup on 1280 ip addresses:
>________________________________________________
>
>18.37.79.51.in-addr.arpa.                86400    IN    PTR      ns1.megacorpone.com.
>...
># =====================================
>```

Using nslookup to perform a simple host enumeration
>``` shell
># Connecting to the Windows 11 client
>kali@kali:~$ xfreerdp /u:student /p:lab /v:192.168.50.152
>
># Using nslookup to perform a simple host enumeration
>C:\Users\student>nslookup mail.megacorptwo.com
>
># ========== Expected Result ==========
>DNS request timed out.
>    timeout was 2 seconds.
>Server:  UnKnown
>Address:  192.168.50.151
>
>Name:    mail.megacorptwo.com
>Address:  192.168.50.154
># =====================================
>
># Using nslookup to perform a more specific query
>C:\Users\student>nslookup -type=TXT info.megacorptwo.com 192.168.50.151
>
>># ========== Expected Result ==========
>Server:  UnKnown
>Address:  192.168.50.151
>
>info.megacorptwo.com    text =
>
>        "greetings from the TXT record body"
># =====================================
>```

Lab 1 - Perform a DNS enumeration on the MX records of megacorpone.com: which is the second-to-best priority value listed in the reply?
>``` shell
>kali@kali:~$ host -t mx megacorpone.com
>
># ========== Expected Result ==========
>megacorpone.com mail is handled by 60 mail2.megacorpone.com.
>megacorpone.com mail is handled by 50 mail.megacorpone.com.
>megacorpone.com mail is handled by 20 spool.mail.gandi.net.
>megacorpone.com mail is handled by 10 fb.mail.gandi.net.
># =====================================
>```
>spool.mail.gandi.net

## TCP/UDP Port Scanning Theory

## Port Scanning with Nmap

## SMB Enumeration

## SMTP Enumeration

## SNMP Enumeration
