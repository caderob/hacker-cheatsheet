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

Using snmpwalk to enumerate the entire MIB tree
>``` shell
>kali@kali:~$ snmpwalk -c public -v1 -t 10 192.168.50.151
>
># ========== Expected Result ==========
>iso.3.6.1.2.1.1.1.0 = STRING: "Hardware: Intel64 Family 6 Model 79 Stepping 1 AT/AT COMPATIBLE - Software: Windows Version 6.3 (Build 17763 Multiprocessor Free)"
>iso.3.6.1.2.1.1.2.0 = OID: iso.3.6.1.4.1.311.1.1.3.1.3
>iso.3.6.1.2.1.1.3.0 = Timeticks: (78235) 0:13:02.35
>iso.3.6.1.2.1.1.4.0 = STRING: "admin@megacorptwo.com"
>iso.3.6.1.2.1.1.5.0 = STRING: "dc01.megacorptwo.com"
>iso.3.6.1.2.1.1.6.0 = ""
>iso.3.6.1.2.1.1.7.0 = INTEGER: 79
>iso.3.6.1.2.1.2.1.0 = INTEGER: 24
>...
># =====================================

Using snmpwalk to enumerate Windows users
>``` shell
>kali@kali:~$ snmpwalk -c public -v1 192.168.50.151 1.3.6.1.4.1.77.1.2.25
>
># ========== Expected Result ==========
>iso.3.6.1.4.1.77.1.2.25.1.1.5.71.117.101.115.116 = STRING: "Guest"
>iso.3.6.1.4.1.77.1.2.25.1.1.6.107.114.98.116.103.116 = STRING: "krbtgt"
>iso.3.6.1.4.1.77.1.2.25.1.1.7.115.116.117.100.101.110.116 = STRING: "student"
>iso.3.6.1.4.1.77.1.2.25.1.1.13.65.100.109.105.110.105.115.116.114.97.116.111.114 = STRING: "Administrator"
># =====================================

Using snmpwalk to enumerate Windows processes
>``` shell
>kali@kali:~$ snmpwalk -c public -v1 192.168.50.151 1.3.6.1.2.1.25.4.2.1.2
>
># ========== Expected Result ==========
>iso.3.6.1.2.1.25.4.2.1.2.1 = STRING: "System Idle Process"
>iso.3.6.1.2.1.25.4.2.1.2.4 = STRING: "System"
>iso.3.6.1.2.1.25.4.2.1.2.88 = STRING: "Registry"
>iso.3.6.1.2.1.25.4.2.1.2.260 = STRING: "smss.exe"
>iso.3.6.1.2.1.25.4.2.1.2.316 = STRING: "svchost.exe"
>iso.3.6.1.2.1.25.4.2.1.2.372 = STRING: "csrss.exe"
>iso.3.6.1.2.1.25.4.2.1.2.472 = STRING: "svchost.exe"
>iso.3.6.1.2.1.25.4.2.1.2.476 = STRING: "wininit.exe"
>iso.3.6.1.2.1.25.4.2.1.2.484 = STRING: "csrss.exe"
>iso.3.6.1.2.1.25.4.2.1.2.540 = STRING: "winlogon.exe"
>iso.3.6.1.2.1.25.4.2.1.2.616 = STRING: "services.exe"
>iso.3.6.1.2.1.25.4.2.1.2.632 = STRING: "lsass.exe"
>iso.3.6.1.2.1.25.4.2.1.2.680 = STRING: "svchost.exe"
>...
># =====================================

Using snmpwalk to enumerate installed software
>``` shell
>kali@kali:~$ snmpwalk -c public -v1 192.168.50.151 1.3.6.1.2.1.25.6.3.1.2
>
># ========== Expected Result ==========
>iso.3.6.1.2.1.25.6.3.1.2.1 = STRING: "Microsoft Visual C++ 2019 X64 Minimum Runtime - 14.27.29016"
>iso.3.6.1.2.1.25.6.3.1.2.2 = STRING: "VMware Tools"
>iso.3.6.1.2.1.25.6.3.1.2.3 = STRING: "Microsoft Visual C++ 2019 X64 Additional Runtime - 14.27.29016"
>iso.3.6.1.2.1.25.6.3.1.2.4 = STRING: "Microsoft Visual C++ 2015-2019 Redistributable (x86) - 14.27.290"
>iso.3.6.1.2.1.25.6.3.1.2.5 = STRING: "Microsoft Visual C++ 2015-2019 Redistributable (x64) - 14.27.290"
>iso.3.6.1.2.1.25.6.3.1.2.6 = STRING: "Microsoft Visual C++ 2019 X86 Additional Runtime - 14.27.29016"
>iso.3.6.1.2.1.25.6.3.1.2.7 = STRING: "Microsoft Visual C++ 2019 X86 Minimum Runtime - 14.27.29016"
>...
># =====================================

Using snmpwalk to enumerate open TCP ports
>``` shell
>kali@kali:~$ snmpwalk -c public -v1 192.168.50.151 1.3.6.1.2.1.6.13.1.3
>
># ========== Expected Result ==========
>iso.3.6.1.2.1.6.13.1.3.0.0.0.0.88.0.0.0.0.0 = INTEGER: 88
>iso.3.6.1.2.1.6.13.1.3.0.0.0.0.135.0.0.0.0.0 = INTEGER: 135
>iso.3.6.1.2.1.6.13.1.3.0.0.0.0.389.0.0.0.0.0 = INTEGER: 389
>iso.3.6.1.2.1.6.13.1.3.0.0.0.0.445.0.0.0.0.0 = INTEGER: 445
>iso.3.6.1.2.1.6.13.1.3.0.0.0.0.464.0.0.0.0.0 = INTEGER: 464
>iso.3.6.1.2.1.6.13.1.3.0.0.0.0.593.0.0.0.0.0 = INTEGER: 593
>iso.3.6.1.2.1.6.13.1.3.0.0.0.0.636.0.0.0.0.0 = INTEGER: 636
>iso.3.6.1.2.1.6.13.1.3.0.0.0.0.3268.0.0.0.0.0 = INTEGER: 3268
>iso.3.6.1.2.1.6.13.1.3.0.0.0.0.3269.0.0.0.0.0 = INTEGER: 3269
>iso.3.6.1.2.1.6.13.1.3.0.0.0.0.5357.0.0.0.0.0 = INTEGER: 5357
>iso.3.6.1.2.1.6.13.1.3.0.0.0.0.5985.0.0.0.0.0 = INTEGER: 5985
>...
># =====================================

Lab 1 - What is the full name of the SNMP server process?
>``` shell
># Scan subnet for live hosts
>kali@kali:~$ sudo nmap -sn 192.168.159.0/24 -oG live.txt
>
># ========== Expected Result ==========
>Starting Nmap 7.95 ( https://nmap.org ) at 2025-10-01 08:30 CDT
>Nmap scan report for 192.168.159.6
>Host is up (0.035s latency).
>Nmap scan report for 192.168.159.8
>Host is up (0.036s latency).
>Nmap scan report for 192.168.159.9
>Host is up (0.035s latency).
>...
># =====================================
>
># Extract IPs
>kali@kali:~$ grep Up live.txt | cut -d" " -f2 > live-hosts.txt
>
># Brute-force SNMP community strings
>kali@kali:~$ onesixtyone -c /usr/share/doc/onesixtyone/dict.txt -i live-hosts.txt
>
># ========== Expected Result ==========
>Scanning 17 hosts, 50 communities
>192.168.159.151 [public] Hardware: AMD64 Family 23 Model 1 Stepping 2 AT/AT COMPATIBLE - Software: Windows Version 6.3 (Build 17763 Multiprocessor Free)
># =====================================
>
># Enumerate running processes via SNMP
>kali@kali:~$ snmpwalk -v1 -c public 192.168.159.151 1.3.6.1.2.1.25.4.2.1.2
>
># ========== Expected Result ==========
>...
>iso.3.6.1.2.1.25.4.2.1.2.2900 = STRING: "svchost.exe"
>iso.3.6.1.2.1.25.4.2.1.2.2908 = STRING: "snmp.exe"
>iso.3.6.1.2.1.25.4.2.1.2.2916 = STRING: "svchost.exe"
>...
># =====================================
>```
>snmp.exe

Lab 2 - Run an SNMP query. What is the first Interface name listed in the output?
>``` shell
># Scan subnet for live hosts
>kali@kali:~$ sudo nmap -sn 192.168.159.0/24 -oG live.txt
>
># ========== Expected Result ==========
>Starting Nmap 7.95 ( https://nmap.org ) at 2025-10-01 08:30 CDT
>Nmap scan report for 192.168.159.6
>Host is up (0.035s latency).
>Nmap scan report for 192.168.159.8
>Host is up (0.036s latency).
>Nmap scan report for 192.168.159.9
>Host is up (0.035s latency).
>...
># =====================================
>
># Extract IPs
>kali@kali:~$ grep Up live.txt | cut -d" " -f2 > live-hosts.txt
>
># Brute-force SNMP community strings
>kali@kali:~$ onesixtyone -c /usr/share/doc/onesixtyone/dict.txt -i live-hosts.txt
>
># ========== Expected Result ==========
>Scanning 17 hosts, 50 communities
>192.168.159.151 [public] Hardware: AMD64 Family 23 Model 1 Stepping 2 AT/AT COMPATIBLE - Software: Windows Version 6.3 (Build 17763 Multiprocessor Free)
># =====================================
>
># Using snmpwalk to enumerate the entire MIB tree and translate any hexadecimal string into ASCII
>kali@kali:~$ snmpwalk -c public -v1 -t 10 -Oa 192.168.159.151 1.3.6.1.2.1.2.2.1.2
>
># ========== Expected Result ==========
>iso.3.6.1.2.1.2.2.1.2.1 = STRING: "Software Loopback Interface 1."
>iso.3.6.1.2.1.2.2.1.2.2 = STRING: "Microsoft 6to4 Adapter."
>iso.3.6.1.2.1.2.2.1.2.3 = STRING: "WAN Miniport (PPTP)."
>...
># =====================================
>```
>Software Loopback Interface 1.
