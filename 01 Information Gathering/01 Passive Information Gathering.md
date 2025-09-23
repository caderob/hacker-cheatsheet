# Passive Information Gathering
### Whois Enumeration
``` shell
# WHOIS forward lookup for a domain
whois <DOMAIN.COM> -h <IP>
```
``` shell
# WHOIS reverse lookup for an IP
whois <TARGET_IP> -h <HOST_IP>
```
Lab 1 - What is the hostname of the third Megacorp One name server?
>``` shell
>whois megacorpone.com -h 192.168.208.251
>```
>Name Server: NS3.MEGACORPONE.COM

Lab 2 - What is the Registrar's WHOIS server?
>``` shell
>whois megacorpone.com -h 192.168.208.251
>```
>Registrar WHOIS Server: whois.gandi.net

Lab 3 - Perform a WHOIS query on the offensive-security.com domain against the machine's IP
>``` shell
>whois -h 192.168.208.251 offensive-security.com
>```
>Name Server: OS{2976c5de7496e2f47f50e70964c64aac}

Lab 4 - Perform a WHOIS query on the kali.org domain against the machine's IP. What's the Tech Email address?
>``` shell
>whois kali.org -h 192.168.208.251
>```
>Tech Email: OS{96ca8ad91db6aef7b47a1a23a1a81e1d}

### Google Hacking
``` shell
# Searching with a Site Operator
Google Search: site:megacorpone.com
```
``` shell
# Searching with a Filetype Operator
Google Search: site:megacorpone.com filetype:txt
```
``` shell
# Searching with the Exclude Operator
Google Search: site:megacorpone.com -filetype:html
```
``` shell
# Using Google to Find Directory Listings
Google Search: site:megacorpone.com intitle:"index of" "parent directory"
```
Lab 1 - What is the name of the VP of Legal for MegaCorp One?
>``` shell
>Google Search: site:megacorpone.com "VP of Legal"
>```
>Name: Mike Carlow

Lab 2 - What is the email address of the VP of Legal for MegaCorp One?
>``` shell
>Google Search: site:megacorpone.com "VP of Legal"
>```
>Email: mcarlow@megacorpone.com 

Lab 3 - What other MegaCorp One employees can you identify that are not listed on www.megacorpone.com?
>``` shell
>Google Search: site:linkedin.com "MegaCorp One"
>```
>Employees at MegaCorp One: Franco Zetticci

### Netcraft (Wappalyzer)
Lab 1 - Determine what application server is running on www.megacorpone.com
>``` shell
>[Google Search: site:linkedin.com "MegaCorp One"](https://www.wappalyzer.com/lookup/megacorpone.com/)
>```
>Web servers: Apache HTTP Server

Lab 2 - What is the name of the Client-Side Scripting Framework that handles fonts?
>``` shell
>[Google Search: site:linkedin.com "MegaCorp One"](https://www.wappalyzer.com/lookup/megacorpone.com/)
>```
>Font Awesome

Lab 3 - What is the value of the IPv4 autonomous systems number that hosts www.megacorpone.com?
>``` shell
># Resolve the IP address of www.megacorpone.com  
nslookup www.megacorpone.com
# Perform a WHOIS lookup using Team Cymru's WHOIS server for ASN info  
whois -h whois.cymru.com " -v <IP_ADDRESS>"
>```
>16276


### Open-Source Code (GitHub, GitHub Gist, GitLab, SourceForge)
``` shell
# Search the 'megacorpone' GitHub account for any files with "users" in the filename
GitHub Search: users in:filename owner:megacorpone
```
Lab 1
``` shell
# Identify user credentials found on Github
https://github.com/megacorpone/megacorpone.com/blob/master/megacorpone/xampp.users
```
Lab 2
``` shell
# Identify the title of the secondary placeholder for the Megacorp One repository
https://github.com/megacorpone
```
