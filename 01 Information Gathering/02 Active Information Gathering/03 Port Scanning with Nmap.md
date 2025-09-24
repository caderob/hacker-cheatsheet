# Active Information Gathering

## Port Scanning with Nmap

Scanning an IP for the 1000 most popular TCP ports
>``` shell
>kali@kali:~$ nmap 192.168.50.149
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-09 05:12 EST
>Nmap scan report for 192.168.50.149
>Host is up (0.10s latency).
>Not shown: 989 closed tcp ports (conn-refused)
>PORT     STATE SERVICE
>53/tcp   open  domain
>88/tcp   open  kerberos-sec
>135/tcp  open  msrpc
>139/tcp  open  netbios-ssn
>389/tcp  open  ldap
>445/tcp  open  microsoft-ds
>464/tcp  open  kpasswd5
>593/tcp  open  http-rpc-epmap
>636/tcp  open  ldapssl
>3268/tcp open  globalcatLDAP
>3269/tcp open  globalcatLDAPssl
>
>Nmap done: 1 IP address (1 host up) scanned in 10.95 seconds
># =====================================
>```

Using nmap to perform a SYN scan
>``` shell
>kali@kali:~$ sudo nmap -sS 192.168.50.149
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-09 06:31 EST
>Nmap scan report for 192.168.50.149
>Host is up (0.11s latency).
>Not shown: 989 closed tcp ports (reset)
>PORT     STATE SERVICE
>53/tcp   open  domain
>88/tcp   open  kerberos-sec
>135/tcp  open  msrpc
>139/tcp  open  netbios-ssn
>389/tcp  open  ldap
>445/tcp  open  microsoft-ds
>464/tcp  open  kpasswd5
>593/tcp  open  http-rpc-epmap
>636/tcp  open  ldapssl
>3268/tcp open  globalcatLDAP
>3269/tcp open  globalcatLDAPssl
>...
># =====================================
>```

Using nmap to perform a TCP connect scan
>``` shell
>kali@kali:~$ nmap -sT 192.168.50.149
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-09 06:44 EST
>Nmap scan report for 192.168.50.149
>Host is up (0.11s latency).
>Not shown: 989 closed tcp ports (conn-refused)
>PORT     STATE SERVICE
>53/tcp   open  domain
>88/tcp   open  kerberos-sec
>135/tcp  open  msrpc
>139/tcp  open  netbios-ssn
>389/tcp  open  ldap
>445/tcp  open  microsoft-ds
>464/tcp  open  kpasswd5
>593/tcp  open  http-rpc-epmap
>636/tcp  open  ldapssl
>3268/tcp open  globalcatLDAP
>3269/tcp open  globalcatLDAPssl
>...
># =====================================
>```

Using nmap to perform a UDP scan
>``` shell
>kali@kali:~$ sudo nmap -sU 192.168.50.149
>
># ========== Expected Result ==========
>Starting Nmap 7.70 ( https://nmap.org ) at 2019-03-04 11:46 EST
>Nmap scan report for 192.168.131.149
>Host is up (0.11s latency).
>Not shown: 977 closed udp ports (port-unreach)
>PORT      STATE         SERVICE
>123/udp   open          ntp
>389/udp   open          ldap
>...
>Nmap done: 1 IP address (1 host up) scanned in 22.49 seconds
># =====================================
>```

Using nmap to perform a combined UDP and SYN scan
>``` shell
>kali@kali:~$ sudo nmap -sU -sS 192.168.50.149
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-09 08:16 EST
>Nmap scan report for 192.168.50.149
>Host is up (0.10s latency).
>Not shown: 989 closed tcp ports (reset), 977 closed udp ports (port-unreach)
>PORT      STATE         SERVICE
>53/tcp    open          domain
>88/tcp    open          kerberos-sec
>135/tcp   open          msrpc
>139/tcp   open          netbios-ssn
>389/tcp   open          ldap
>445/tcp   open          microsoft-ds
>464/tcp   open          kpasswd5
>593/tcp   open          http-rpc-epmap
>636/tcp   open          ldapssl
>3268/tcp  open          globalcatLDAP
>3269/tcp  open          globalcatLDAPssl
>53/udp    open          domain
>123/udp   open          ntp
>389/udp   open          ldap
>...
># =====================================
>```
