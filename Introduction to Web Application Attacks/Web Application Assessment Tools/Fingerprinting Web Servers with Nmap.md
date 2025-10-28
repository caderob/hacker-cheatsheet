# Fingerprinting Web Servers with Nmap

Running Nmap scan to discover web server version
>``` shell
>kali@kali:~$ sudo nmap -p80  -sV 192.168.50.20
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-29 05:13 EDT
>Nmap scan report for 192.168.50.20
>Host is up (0.11s latency).
>
>PORT   STATE SERVICE VERSION
>80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
># =====================================
>```

Running Nmap NSE http enumeration script against the target
>``` shell
>kali@kali:~$ sudo nmap -p80 --script=http-enum 192.168.50.20
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-29 06:30 EDT
>Nmap scan report for 192.168.50.20
>Host is up (0.10s latency).
>
>PORT   STATE SERVICE
>80/tcp open  http
>| http-enum:
>|   /login.php: Possible admin folder
>|   /db/: BlogWorx Database
>|   /css/: Potentially interesting directory w/ listing on 'apache/2.4.41 (ubuntu)'
>|   /db/: Potentially interesting directory w/ listing on 'apache/2.4.41 (ubuntu)'
>|   /images/: Potentially interesting directory w/ listing on 'apache/2.4.41 (ubuntu)'
>|   /js/: Potentially interesting directory w/ listing on 'apache/2.4.41 (ubuntu)'
>|_  /uploads/: Potentially interesting directory w/ listing on 'apache/2.4.41 (ubuntu)'
>
>Nmap done: 1 IP address (1 host up) scanned in 16.82 seconds
># =====================================
>```
