# Passive Information Gathering

## Whois Enumeration

WHOIS forward lookup for megacorpone.com
>``` shell
>kali@kali:~$ whois megacorpone.com -h 192.168.50.251
>
># === Expected Result ===
>Domain Name: MEGACORPONE.COM
>Registry Domain ID: 1775445745_DOMAIN_COM-VRSN
>Registrar WHOIS Server: whois.gandi.net
>Registrar URL: http://www.gandi.net
>Updated Date: 2019-01-01T09:45:03Z
>Creation Date: 2013-01-22T23:01:00Z
>Registry Expiry Date: 2023-01-22T23:01:00Z
>...
>Registry Registrant ID: 
>Registrant Name: Alan Grofield
>Registrant Organization: MegaCorpOne
>Registrant Street: 2 Old Mill St
>Registrant City: Rachel
>Registrant State/Province: Nevada
>Registrant Postal Code: 89001
>Registrant Country: US
>Registrant Phone: +1.9038836342
>...
>Registry Admin ID: 
>Admin Name: Alan Grofield
>Admin Organization: MegaCorpOne
>Admin Street: 2 Old Mill St
>Admin City: Rachel
>Admin State/Province: Nevada
>Admin Postal Code: 89001
>Admin Country: US
>Admin Phone: +1.9038836342
>...
>Registry Tech ID: 
>Tech Name: Alan Grofield
>Tech Organization: MegaCorpOne
>Tech Street: 2 Old Mill St
>Tech City: Rachel
>Tech State/Province: Nevada
>Tech Postal Code: 89001
>Tech Country: US
>Tech Phone: +1.9038836342
>...
>Name Server: NS1.MEGACORPONE.COM
>Name Server: NS2.MEGACORPONE.COM
>Name Server: NS3.MEGACORPONE.COM
>...
># =======================
>```

WHOIS reverse lookup for IP 38.100.193.70
>``` shell
>kali@kali:~$ whois 38.100.193.70 -h 192.168.50.251
>
># === Expected Result ===
>...
>NetRange:       38.0.0.0 - 38.255.255.255
>CIDR:           38.0.0.0/8
>NetName:        COGENT-A
>...
>OrgName:        PSINet, Inc.
>OrgId:          PSI
>Address:        2450 N Street NW
>City:           Washington
>StateProv:      DC
>PostalCode:     20037
>Country:        US
>RegDate:
>Updated:        2015-06-04
>...
># =======================
>```

Lab 1 - What is the hostname of the third Megacorp One name server?
>``` shell
>kali@kali:~$ whois megacorpone.com -h 192.168.208.251
>```
>Name Server: NS3.MEGACORPONE.COM

Lab 2 - What is the Registrar's WHOIS server?
>``` shell
>kali@kali:~$ whois megacorpone.com -h 192.168.208.251
>```
>Registrar WHOIS Server: whois.gandi.net

Lab 3 - Perform a WHOIS query on the offensive-security.com domain against the machine's IP
>``` shell
>kali@kali:~$ whois -h 192.168.208.251 offensive-security.com
>```
>Name Server: OS{2976c5de7496e2f47f50e70964c64aac}

Lab 4 - Perform a WHOIS query on the kali.org domain against the machine's IP. What's the Tech Email address?
>``` shell
>kali@kali:~$ whois kali.org -h 192.168.208.251
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

## Netcraft (Wappalyzer)
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
>kali@kali:~$ nslookup www.megacorpone.com
>
># === Expected Result ===
>Server:         192.168.1.1
>Address:        192.168.1.1#53
>
>Non-authoritative answer:
>Name:   www.megacorpone.com
>Address: 149.56.244.87
># =======================
>
># Perform a WHOIS lookup using a public server (Team Cymru)  
>kali@kali:~$ whois -h whois.cymru.com " -v 149.56.244.87"
>
># === Expected Result ===
>AS      | IP               | BGP Prefix          | CC | Registry | Allocated  | AS Name
>16276   | 149.56.244.87    | 149.56.0.0/16       | CA | arin     | 2016-02-09 | OVH, FR
># =======================
>```
>16276

## Open-Source Code (GitHub, GitHub Gist, GitLab, SourceForge)

Search the 'megacorpone' GitHub account for any files with "users" in the filename
>``` shell
>GitHub Search: users in:filename owner:megacorpone
>```

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
