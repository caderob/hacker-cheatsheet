# Active Information Gathering

## SMTP Enumeration

Using nc to validate SMTP users
>``` shell
>kali@kali:~$ nc -nv 192.168.50.8 25
>
># ========== Expected Result ==========
>(UNKNOWN) [192.168.50.8] 25 (smtp) open
>220 mail ESMTP Postfix (Ubuntu)
>VRFY root
>252 2.0.0 root
>VRFY idontexist
>550 5.1.1 <idontexist>: Recipient address rejected: User unknown in local recipient table
>^C
># =====================================

Using Python to script the SMTP user enumeration
>``` python
>#!/usr/bin/python
>
>import socket
>import sys
>
>if len(sys.argv) != 3:
>        print("Usage: vrfy.py <username> <target_ip>")
>        sys.exit(0)
>
># Create a Socket
>s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
>
># Connect to the Server
>ip = sys.argv[2]
>connect = s.connect((ip,25))
>
># Receive the banner
>banner = s.recv(1024)
>
>print(banner)
>
># VRFY a user
>user = (sys.argv[1]).encode()
>s.send(b'VRFY ' + user + b'\r\n')
>result = s.recv(1024)
>
>print(result)
>
># Close the socket
>s.close()
>```

Running the Python script to perform SMTP user enumeration
>``` shell
>kali@kali:~/Desktop$ python3 smtp.py root 192.168.50.8
># ========== Expected Result ==========
>b'220 mail ESMTP Postfix (Ubuntu)\r\n'
>b'252 2.0.0 root\r\n'
># =====================================
>
>kali@kali:~/Desktop$ python3 smtp.py johndoe 192.168.50.8
># ========== Expected Result ==========
>b'220 mail ESMTP Postfix (Ubuntu)\r\n'
>b'550 5.1.1 <johndoe>: Recipient address rejected: User unknown in local recipient table\r\n'
># =====================================
>```
