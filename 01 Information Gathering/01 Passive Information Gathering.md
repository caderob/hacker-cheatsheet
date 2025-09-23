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
Lab 1 
``` shell
# What is the hostname of the third Megacorp One name server?

whois megacorpone.com -h 192.168.208.251
# Name Server: NS3.MEGACORPONE.COM
```
Lab 2
``` shell
# What is the Registrar's WHOIS server?

whois megacorpone.com -h 192.168.208.251
# Registrar WHOIS Server: whois.gandi.net
```
Lab 3
``` shell
# Perform a WHOIS query on the offensive-security.com domain against the machine's IP

whois -h 192.168.208.251 offensive-security.com
# Name Server: OS{2976c5de7496e2f47f50e70964c64aac}
```
Lab 4
``` shell
# Perform a WHOIS query on the kali.org domain against the machine's IP. What's the Tech Email address?

whois kali.org -h 192.168.208.251
# Tech Email: OS{96ca8ad91db6aef7b47a1a23a1a81e1d}
```
### Google Hacking
```
# Searching with a Site Operator
Google Search: site:megacorpone.com
```
```
# Searching with a Filetype Operator
Google Search: site:megacorpone.com filetype:txt
```
```
# Searching with the Exclude Operator
Google Search: site:megacorpone.com -filetype:html
```
```
# Using Google to Find Directory Listings
Google Search: site:megacorpone.com intitle:"index of" "parent directory"
```
Lab 1
```
# What is the name of the VP of Legal for MegaCorp One?

Google Search: site:megacorpone.com "VP of Legal"
# Name: Mike Carlow
```
Lab 2
```
# What is the email address of the VP of Legal for MegaCorp One?

Google Search: site:megacorpone.com "VP of Legal"
# Email: mcarlow@megacorpone.com 
```
Lab 3
```
# What other MegaCorp One employees can you identify that are not listed on www.megacorpone.com?

Google Search: site:linkedin.com "MegaCorp One"
# Employees at MegaCorp One: Franco Zetticci
```
### Netcraft (Wappalyzer)
Lab 1 & 2
```
# Identify technologies (Application Server and CSS Font Framework) for a specific domain (megacorpone.com)
https://www.wappalyzer.com/lookup/megacorpone.com/
```
Lab 3
```
# Identify the IPv4 autonomous systems number that hosts a domain (megacorpone.com)
# Resolve the IP address of www.megacorpone.com  
nslookup www.megacorpone.com
# Perform a WHOIS lookup using Team Cymru's WHOIS server for ASN info  
whois -h whois.cymru.com " -v <IP_ADDRESS>"
```
### Open-Source Code (GitHub, GitHub Gist, GitLab, SourceForge)
```
# Search the 'megacorpone' GitHub account for any files with "users" in the filename
GitHub Search: users in:filename owner:megacorpone
```
Lab 1
```
# Identify user credentials found on Github
https://github.com/megacorpone/megacorpone.com/blob/master/megacorpone/xampp.users
```
Lab 2
```
# Identify the title of the secondary placeholder for the Megacorp One repository
https://github.com/megacorpone
```
