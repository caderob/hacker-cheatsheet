# Connecting to the PWK Lab

The kali terminal
>``` shell
>┌──(kali㉿kali)-[~]
>└─$   
>```

The kali terminal with a different username
>``` shell
>┌──(ArtVandelay㉿kali)-[~]
>└─$   
>```

Switching to the one-line command prompt
>``` shell
>kali@kali:~$ 
>```

Running updatedb
>``` shell
>kali@kali:~$ sudo updatedb
>```

Finding the .ovpn file
>``` shell
>kali@kali:~$ locate universal.ovpn
>
># ========== Expected Result ==========
>/home/kali/Downloads/universal.ovpn
># =====================================
>```

Changing Directories with cd
>``` shell
>kali@kali:~$ cd /home/kali/Downloads         
>
>kali@kali:~/Downloads$ 
>```

Listing file contents with ls
>``` shell
>kali@kali:~/Downloads$ ls   
>
># ========== Expected Result ==========
>universal.ovpn
># =====================================
>```

Creating a new directory and moving the .ovpn file
>``` shell
>kali@kali:~/Downloads$ mkdir /home/kali/offsec
>
>kali@kali:~/Downloads$ mv universal.ovpn /home/kali/offsec/universal.ovpn
>
>kali@kali:~/Downloads$ cd ../offsec
>
>kali@kali:~/offsec$
>```

Connecting to the labs VPN
>``` shell
>kali@kali:~/offsec$ sudo openvpn universal.ovpn 
>
># ========== Expected Result ==========
>2021-06-28 10:20:12 Note: Treating option '--ncp-ciphers' as  '--data-ciphers' (renamed in OpenVPN 2.5).
>2021-06-28 10:20:12 DEPRECATED OPTION: --cipher set to 'AES-128-CBC' but missing in --data-ciphers (AES-128-GCM). Future OpenVPN version will ignore --cipher for cipher negotiations. Add 'AES-128-CBC' to --data-ciphers or change --cipher 'AES-128-CBC' to --data-ciphers-fallback 'AES-128-CBC' to silence this warning.
>2021-06-28 10:20:12 OpenVPN 2.5.1 x86_64-pc-linux-gnu [SSL (OpenSSL)] [LZO] [LZ4] [EPOLL] [PKCS11] [MH/PKTINFO] [AEAD] built on May 14 2021
>2021-06-28 10:20:12 library versions: OpenSSL 1.1.1k  25 Mar 2021, LZO 2.10
>2021-06-28 10:20:12 TCP/UDP: Preserving recently used remote address: [AF_INET]192.95.19.165:1194
>2021-06-28 10:20:12 UDP link local: (not bound)
>2021-06-28 10:20:12 UDP link remote: [AF_INET]192.95.19.165:1194
>2021-06-28 10:20:12 [offsec.com] Peer Connection Initiated with [AF_INET]192.95.19.165:1194
>2021-06-28 10:20:13 TUN/TAP device tun0 opened
>2021-06-28 10:20:13 net_iface_mtu_set: mtu 1500 for tun0
>2021-06-28 10:20:13 net_iface_up: set tun0 up
>2021-06-28 10:20:13 net_addr_v4_add: 192.168.49.115/24 dev tun0
>2021-06-28 10:20:13 WARNING: this configuration may cache passwords in memory -- use the auth-nocache option to prevent this
>2021-06-28 10:20:13 Initialization Sequence Completed
># =====================================
>```
