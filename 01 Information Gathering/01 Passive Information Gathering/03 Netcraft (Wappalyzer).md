# Passive Information Gathering

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
># ========== Expected Result ==========
>Server:         192.168.1.1
>Address:        192.168.1.1#53
>
>Non-authoritative answer:
>Name:   www.megacorpone.com
>Address: 149.56.244.87
># =====================================
>
># Perform a WHOIS lookup using a public server (Team Cymru)  
>kali@kali:~$ whois -h whois.cymru.com " -v 149.56.244.87"
>
># ========== Expected Result ==========
>AS      | IP               | BGP Prefix          | CC | Registry | Allocated  | AS Name
># =====================================
>```
>16276
