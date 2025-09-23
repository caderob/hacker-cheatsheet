# Passive Information Gathering
## Whois Enumeration

WHOIS forward lookup for a domain
>``` shell
>whois <DOMAIN.COM> -h <IP>
>```

WHOIS reverse lookup for an IP
>``` shell
>whois <TARGET_IP> -h <HOST_IP>
>```

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

## Google Hacking

Searching with a Site Operator
>``` shell
>Google Search: site:megacorpone.com
>```

Searching with a Filetype Operator
>``` shell
>Google Search: site:megacorpone.com filetype:txt
>```

Searching with the Exclude Operator
>``` shell
>Google Search: site:megacorpone.com -filetype:html
>```

Using Google to Find Directory Listings
>``` shell
>Google Search: site:megacorpone.com intitle:"index of" "parent directory"
>```

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
>https://www.wappalyzer.com/lookup/megacorpone.com/
>```
>Web servers: Apache HTTP Server

Lab 2 - What is the name of the Client-Side Scripting Framework that handles fonts?
>``` shell
>https://www.wappalyzer.com/lookup/megacorpone.com/
>```
>Font Awesome

Lab 3 - What is the value of the IPv4 autonomous systems number that hosts www.megacorpone.com?
>``` shell
># Resolve the IP address of www.megacorpone.com  
>nslookup www.megacorpone.com
># Perform a WHOIS lookup using a public server (Team Cymru)  
>whois -h whois.cymru.com " -v 149.56.244.87" 
>```
>16276


### Open-Source Code (GitHub, GitHub Gist, GitLab, SourceForge)
``` shell
# Search the 'megacorpone' GitHub account for any files with "users" in the filename
GitHub Search: users in:filename owner:megacorpone
```
Lab 1 - Find a useraname using open-source GitHub
>``` shell
>https://github.com/megacorpone/megacorpone.com/blob/master/megacorpone/xampp.users
>```
>trivera

Lab 2 - What is the title of the secondary, placeholder, Megacorp One repository?
>``` shell
>https://github.com/megacorpone
>```
>git-test
