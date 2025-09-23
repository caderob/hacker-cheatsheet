# Passive Information Gathering
### Whois Enumeration
``` shell
# WHOIS forward lookup for a domain
whois <DOMAIN.COM> -h <IP>
```
``` shell
# WHOIS reverse lookup for IP
whois <TARGET_IP> -h <HOST_IP>
```
Lab 1 & 2
``` shell
# Query domain registration information for megacorpone.com using custom WHOIS server  
whois megacorpone.com -h <WHOIS_SERVER_IP>
```
Lab 3
``` shell
# Query domain registration info for offensive-security.com  
whois -h <WHOIS_SERVER_IP> offensive-security.com
```
Lab 4
``` shell
# Query domain registration info for kali.org  
whois kali.org -h <WHOIS_SERVER_IP>
```
### Google Hacking
```
# Searching with a Site Operator
Google: site:megacorpone.com
```
```
# Searching with a Filetype Operator
Google: site:megacorpone.com filetype:txt
```
```
# Searching with the Exclude Operator
Google: site:megacorpone.com -filetype:html
```
```
# Using Google to Find Directory Listings
Google: site:megacorpone.com intitle:"index of" "parent directory"
```
Lab 1 & 2
```
# Search megacorpone.com for the words "VP of Legal"
Google: site:megacorpone.com "VP of Legal"
```
Lab 3
```
# Search linkedin.com for the words "MegaCorp One"
Google: site:linkedin.com "MegaCorp One"
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
