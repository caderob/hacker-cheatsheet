# Active Information Gathering

## SNMP Enumeration

Using nmap to perform a SNMP scan
>``` shell
>kali@kali:~$ sudo nmap -sU --open -p 161 192.168.50.1-254 -oG open-snmp.txt
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-14 06:02 EDT
>Nmap scan report for 192.168.50.151
>Host is up (0.10s latency).
>
>PORT    STATE SERVICE
>161/udp open  snmp
>
>Nmap done: 1 IP address (1 host up) scanned in 0.49 seconds
>...
># =====================================

Using nmap to perform a SNMP scan
>``` shell
>kali@kali:~$ echo public > community
>kali@kali:~$ echo private >> community
>kali@kali:~$ echo manager >> community
>
>kali@kali:~$ for ip in $(seq 1 254); do echo 192.168.50.$ip; done > ips
>
>kali@kali:~$ onesixtyone -c community -i ips
>
># ========== Expected Result ==========
>Scanning 254 hosts, 3 communities
>192.168.50.151 [public] Hardware: Intel64 Family 6 Model 79 Stepping 1 AT/AT COMPATIBLE - Software: Windows Version 6.3 (Build 17763 Multiprocessor Free)
>...
># =====================================
