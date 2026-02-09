# Loading Jenkins

Tunneling Diagram
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Loading-Jenkins-1.png)

Exiting Shell and Sending Session to Background
>``` shell
>exit
>
># ========== Expected Result ==========
>[-] core_channel_interact: Operation failed: Unknown error
># =====================================
>
>meterpreter > background
>
># ========== Expected Result ==========
>[*] Backgrounding session 1...
># =====================================
>```

Using SOCKS Proxy Module and Running it
>``` shell
>msf6 exploit(multi/handler) > use auxiliary/server/socks_proxy
>
>msf6 auxiliary(server/socks_proxy) > set SRVHOST 127.0.0.1
>
># ========== Expected Result ==========
>SRVHOST => 127.0.0.1
># =====================================
>
>msf6 auxiliary(server/socks_proxy) > run -j
>
># ========== Expected Result ==========
>[*] Auxiliary module running as background job 1.
># =====================================
>```

Creating Route
>``` shell
>msf6 exploit(server/socks_proxy) > sessions
>
># ========== Expected Result ==========
>Active sessions
>===============
>
>  Id  Name  Type                      Information          Connection
>  --  ----  ----                      -----------          ----------
>  2         meterpreter python/linux  root @ 6699d104d6c5  10.0.1.54:4488 -> 198.18.53.73:37604 (172.18.0.4)
># =====================================
>
>msf6 auxiliary(server/socks_proxy) > route add 172.30.0.1 255.255.0.0 2
>```

Tunneling Diagram with No SSH Tunnel
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Loading-Jenkins-2.png)

Creating Local Forward SSH Tunnel
>``` shell
>kali@kali:~$ ssh -fN -L localhost:1080:localhost:1080 kali@192.88.99.76
>
># ========== Expected Result ==========
>kali@192.88.99.76's password:
># =====================================
>
>kali@kali:~$ ss -tulpn
>
># ========== Expected Result ==========
>Netid  State   Recv-Q  Send-Q   Local Address:Port   Peer Address:Port Process                          
>tcp    LISTEN  0       128          127.0.0.1:1080        0.0.0.0:*     users:(("ssh",pid=75991,fd=5))  
>tcp    LISTEN  0       128              [::1]:1080           [::]:*     users:(("ssh",pid=75991,fd=4))  
># =====================================
>```

Opening Settings in Firefox
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Loading-Jenkins-3.png)

Opening Network Settings In Firefox
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Loading-Jenkins-4.png)

Add Socks Proxy to Firefox
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Loading-Jenkins-5.png)

Jenkins in Firefox
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Loading-Jenkins-6.png)
