# Information Gathering

  - # Passive Information Gathering

    - # Whois Enumeration

WHOIS forward lookup for megacorpone.com
>``` shell
>kali@kali:~$ whois megacorpone.com -h 192.168.50.251
>
># ========== Expected Result ==========
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
># =====================================
>```

WHOIS reverse lookup for IP 38.100.193.70
>``` shell
>kali@kali:~$ whois 38.100.193.70 -h 192.168.50.251
>
># ========== Expected Result ==========
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
># =====================================
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
