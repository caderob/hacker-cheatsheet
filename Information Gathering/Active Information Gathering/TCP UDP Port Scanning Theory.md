# TCP/UDP Port Scanning Theory

Using netcat to perform a TCP port scan
>``` shell
>kali@kali:~$ nc -nvv -w 1 -z 192.168.50.152 3388-3390
>
># ========== Expected Result ==========
>(UNKNOWN) [192.168.50.152] 3390 (?) : Connection refused
>(UNKNOWN) [192.168.50.152] 3389 (ms-wbt-server) open
>(UNKNOWN) [192.168.50.152] 3388 (?) : Connection refused
> sent 0, rcvd 0
># =====================================
>```

Using Netcat to perform a UDP port scan
>``` shell
>kali@kali:~$ nc -nvv -u -z -w 1 192.168.50.149 120-123
>
># ========== Expected Result ==========
>(UNKNOWN) [192.168.50.149] 123 (ntp) open
># =====================================
>```

Lab 1 - Once VM Group 1 is started, perform a Netcat scan against the machine ending with the octet '151' (ex: 192.168.51.151) Which is the lowest TCP open port?
>``` shell
>kali@kali:~$ nc -nvv -w 1 -z 192.168.170.151 1-100
>
># ========== Expected Result ==========
>...
>(UNKNOWN) [192.168.170.151] 56 (?) : Connection refused
>(UNKNOWN) [192.168.170.151] 55 (?) : Connection refused
>(UNKNOWN) [192.168.170.151] 54 (?) : Connection refused
>(UNKNOWN) [192.168.170.151] 53 (domain) open
>(UNKNOWN) [192.168.170.151] 52 (?) : Connection refused
>(UNKNOWN) [192.168.170.151] 51 (?) : Connection refused
>(UNKNOWN) [192.168.170.151] 50 (?) : Connection refused
>...
># =====================================
>```
>53

Lab 2 - On the same host, perform a netcat TCP scan for the port range 1-10000. Which is the highest open TCP port?
>``` shell
>kali@kali:~$ nc -nvv -w 1 -z 192.168.170.151 9000-10000
>
># ========== Expected Result ==========
>...
>(UNKNOWN) [192.168.170.151] 9392 (?) : Connection refused
>(UNKNOWN) [192.168.170.151] 9391 (?) : Connection refused
>(UNKNOWN) [192.168.170.151] 9390 (?) : Connection refused
>(UNKNOWN) [192.168.170.151] 9389 (?) open
>(UNKNOWN) [192.168.170.151] 9388 (?) : Connection refused
>(UNKNOWN) [192.168.170.151] 9387 (?) : Connection refused
>(UNKNOWN) [192.168.170.151] 9386 (?) : Connection refused
>...
># =====================================
>```
>9389

Lab 3 - Other than port 123, what is the first returned open UDP port in the range 150-200 when scanning the machine ending with the octet '151' (ex: 192.168.51.151)?
>``` shell
>kali@kali:~$ nc -nvv -u -z -w 1 192.168.170.151 150-200
>
># ========== Expected Result ==========
>...
>(UNKNOWN) [192.168.170.151] 164 (cmip-agent) : Connection refused
>(UNKNOWN) [192.168.170.151] 163 (cmip-man) : Connection refused
>(UNKNOWN) [192.168.170.151] 162 (snmp-trap) : Connection refused
>(UNKNOWN) [192.168.170.151] 161 (snmp) open
>(UNKNOWN) [192.168.170.151] 160 (?) : Connection refused
>(UNKNOWN) [192.168.170.151] 159 (?) : Connection refused
>(UNKNOWN) [192.168.170.151] 158 (?) : Connection refused
>...
># =====================================
>```
>161
