# Domain and Subdomain Reconnaissance

Querying Nameserver Records of offseclab.io Domain
>``` shell
>kali@kali:~$ host -t ns offseclab.io
>
># ========== Expected Result ==========
>offseclab.io name server ns-1536.awsdns-00.co.uk.
>offseclab.io name server ns-512.awsdns-00.net.
>offseclab.io name server ns-0.awsdns-00.com.
>offseclab.io name server ns-1024.awsdns-00.org.
># =====================================
>```

Getting the Registrar Information of awsdns-00.com Domain
>``` shell
>kali@kali:~$ whois awsdns-00.com | grep "Registrant Organization"
>
># ========== Expected Result ==========
>Registrant Organization: Amazon Technologies, Inc.
># =====================================
>```

Getting the Public IP address of www.offseclab.io
>``` shell
>kali@kali:~$ host www.offseclab.io
>
># ========== Expected Result ==========
>www.offseclab.io has address 52.70.117.69
># =====================================
>```

Getting Details of the Public IP Address of the Website
>``` shell
>kali@kali:~$ host 52.70.117.69
>
># ========== Expected Result ==========
>69.117.70.52.in-addr.arpa domain name pointer ec2-52-70-117-69.compute-1.amazonaws.com
># =====================================
>
>kali@kali:~$ whois 52.70.117.69 | grep "OrgName"
>
># ========== Expected Result ==========
>OrgName:        Amazon Technologies Inc.
># =====================================
>```

Using dnsenum to Automate DNS Reconnaissance of offseclab.io Domain
>``` shell
>kali@kali:~$ dnsenum offseclab.io --threads 100
>
># ========== Expected Result ==========
>dnsenum VERSION:1.2.6
>
>-----   offseclab.io   -----
>
>
>Host's addresses:
>__________________
>
>offseclab.io.                            60       IN    A        52.70.117.69
>
>Name Servers:
>______________
>
>ns-1536.awsdns-00.co.uk.                 0        IN    A        205.251.198.0
>ns-0.awsdns-00.com.                      0        IN    A        205.251.192.0
>ns-512.awsdns-00.net.                    0        IN    A        205.251.194.0
>ns-1024.awsdns-00.org.                   0        IN    A        205.251.196.0
>
>
>Mail (MX) Servers:
>___________________
>
>
>
>Trying Zone Transfers and getting Bind Versions:
>_________________________________________________
>
>Trying Zone Transfer for offseclab.io on ns-512.awsdns-00.net ...
>AXFR record query failed: corrupt packet
>
>Trying Zone Transfer for offseclab.io on ns-1024.awsdns-00.org ...
>AXFR record query failed: corrupt packet
>
>Trying Zone Transfer for offseclab.io on ns-0.awsdns-00.com ...
>AXFR record query failed: corrupt packet
>
>Trying Zone Transfer for offseclab.io on ns-1536.awsdns-00.co.uk ...
>AXFR record query failed: corrupt packet
>
>
>Brute forcing with /usr/share/dnsenum/dns.txt:
>_______________________________________________
>mail.offseclab.io.                       60       IN    A        52.70.117.69
>www.offseclab.io.                        60       IN    A        52.70.117.69
>...
># =====================================
>```

Lab 1 - What command is used to query the authoritative DNS servers for the domain offseclab.io?
>A) host -t ns offseclab.io

Lab 2 - Which AWS service is very likely being used to manage the offseclab.io domain?
>C) Amazon Route 53

Lab 3 - Find the proof while gathering more info about the domain inside other commonly used DNS records.
>
