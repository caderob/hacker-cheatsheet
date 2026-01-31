# Netsh

The network setup for this scenario
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Netsh-1.png)

Connecting to the RDP server with xfreerdp
>``` shell
>kali@kali:~$ xfreerdp /u:rdp_admin /p:P@ssw0rd! /v:192.168.50.64
>
># ========== Expected Result ==========
>[07:48:02:576] [265164:265165] [WARN][com.freerdp.crypto] - Certificate verification failure 'self signed certificate (18)' at stack position 0
>[07:48:02:577] [265164:265165] [WARN][com.freerdp.crypto] - CN = MULTISERVER03
>[07:48:03:685] [265164:265165] [ERROR][com.winpr.timezone] - Unable to find a match for unix timezone: US/Eastern
>[07:48:03:886] [265164:265165] [INFO][com.freerdp.gdi] - Local framebuffer format  PIXEL_FORMAT_BGRX32
>[07:48:03:886] [265164:265165] [INFO][com.freerdp.gdi] - Remote framebuffer format PIXEL_FORMAT_BGRA32
>[07:48:03:940] [265164:265165] [INFO][com.freerdp.channels.rdpsnd.client] - [static] Loaded fake backend for rdpsnd
>[07:48:03:940] [265164:265165] [INFO][com.freerdp.channels.drdynvc.client] - Loading Dynamic Virtual Channel rdpgfx
># =====================================
>```

The portproxy command being run
>``` shell
>C:\Windows\system32>netsh interface portproxy add v4tov4 listenport=2222 listenaddress=192.168.50.64 connectport=22 connectaddress=10.4.50.215
>```

netstat showing that TCP/2222 is listening on the external interface
>``` shell
>C:\Windows\system32>netstat -anp TCP | find "2222"
>
># ========== Expected Result ==========
>  TCP    192.168.50.64:2222     0.0.0.0:0              LISTENING
># =====================================
>```

Listing all the portproxy port forwarders set up with Netsh
>``` shell
>C:\Windows\system32>netsh interface portproxy show all
>
># ========== Expected Result ==========
>Listen on ipv4:             Connect to ipv4:
>
>Address         Port        Address         Port
>--------------- ----------  --------------- ----------
>192.168.50.64   2222        10.4.50.215     22
># =====================================
>```

The port forward set up on MULTISERVER03 will forward packets recieved on port 2222 to port 22 on PGDATABASE01
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Netsh-2.png)

Nmap showing that port 2222 on MULTISERVER03 is filtered
>``` shell
>kali@kali:~$ sudo nmap -sS 192.168.50.64 -Pn -n -p2222
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-21 06:27 EDT
>Nmap scan report for 192.168.50.64
>Host is up (0.00055s latency).
>
>PORT     STATE    SERVICE
>2222/tcp filtered EtherNetIP-1
>MAC Address: 00:0C:29:A9:9F:3D (VMware)
>
>Nmap done: 1 IP address (1 host up) scanned in 0.50 seconds
># =====================================
>```

The Windows firewall blocking our attempt to connect to port 2222 on MULTISERVER03 from our Kali machine on the WAN network
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Netsh-3.png)

Poking a hole in the Windows Firewall with Netsh
>``` shell
>C:\Windows\system32> netsh advfirewall firewall add rule name="port_forward_ssh_2222" protocol=TCP dir=in localip=192.168.50.64 localport=2222 action=allow
>
># ========== Expected Result ==========
>Ok.
># =====================================
>```

Nmap showing that port 2222 on MULTISERVER03 is open
>``` shell
>kali@kali:~$ sudo nmap -sS 192.168.50.64 -Pn -n -p2222
>
># ========== Expected Result ==========
>Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-21 06:28 EDT
>Nmap scan report for 192.168.50.64
>Host is up (0.00060s latency).
>
>PORT     STATE SERVICE
>2222/tcp open  EtherNetIP-1
>MAC Address: 00:0C:29:A9:9F:3D (VMware)
>
>Nmap done: 1 IP address (1 host up) scanned in 0.23 seconds
># =====================================
>```

SSHing into PGDATABASE01 through the Netsh port forward
>``` shell
>kali@kali:~$ ssh database_admin@192.168.50.64 -p2222
>
># ========== Expected Result ==========
>The authenticity of host '[192.168.50.64]:2222 ([192.168.50.64]:2222)' can't be established.
>ED25519 key fingerprint is SHA256:3TRC1ZwtlQexLTS04hV3ZMbFn30lYFuQVQHjUqlYzJo.
>This host key is known by the following other names/addresses:
>    ~/.ssh/known_hosts:5: [hashed name]
>Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
>Warning: Permanently added '[192.168.50.64]:2222' (ED25519) to the list of known hosts.
>database_admin@192.168.50.64's password: 
>Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-122-generic x86_64)
>
> * Documentation:  https://help.ubuntu.com
> * Management:     https://landscape.canonical.com
> * Support:        https://ubuntu.com/advantage
>
>  System information as of Sun 21 Aug 2022 10:40:26 PM UTC
>
>  System load:  0.0               Processes:               231
>  Usage of /:   60.9% of 7.77GB   Users logged in:         0
>  Memory usage: 16%               IPv4 address for ens192: 10.4.50.215
>  Swap usage:   0%                IPv4 address for ens224: 172.16.50.215
>
>
>0 updates can be applied immediately.
>
>
>Last login: Sat Aug 20 21:47:47 2022 from 10.4.50.63
>database_admin@pgdatabase01:~$
># =====================================
>```

Connecting to port 2222 on MULTISERVER03 through the hole we made in the Windows firewall
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Netsh-4.png)

Deleting the firewall rule with Netsh
>``` shell
>C:\Users\Administrator>netsh advfirewall firewall delete rule name="port_forward_ssh_2222"
>
># ========== Expected Result ==========
>Deleted 1 rule(s).
>Ok.
># =====================================
>```

Deleting the port forwarding rule with Netsh
>``` shell
>C:\Windows\Administrator> netsh interface portproxy del v4tov4 listenport=2222 listenaddress=192.168.50.64
>```

Lab 1 - Start VM Group 1. As in the walkthrough, RDP into MULTISERVER03 and create a port forward with Netsh, in order to SSH into PGDATABASE01 from the Kali machine. Retrieve the flag on PGDATABASE01 at /tmp/netsh_flag.
>``` shell
>
>```
>

Lab 2 - Capstone Lab: Start VM Group 2. Download the netsh_exercise_client binary from the Resources section to your Kali machine. If you're running the aarch64 build of Kali, download the netsh_exercise_client_aarch64 binary. Create a port forward on MULTISERVER03 that allows you to run this binary against port 4545 on PGDATABASE01. The flag will be returned when a successful connection is made.
>``` shell
>
>```
>
