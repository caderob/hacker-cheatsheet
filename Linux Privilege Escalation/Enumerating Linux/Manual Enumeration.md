# Manual Enumeration

Getting information about the current user
>``` shell
>joe@debian-privesc:~$ id 
>
># ========== Expected Result ==========
>uid=1000(joe) gid=1000(joe) groups=1000(joe),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),109(netdev),112(bluetooth),116(lpadmin),117(scanner)
># =====================================
>```

Getting information about the users
>``` shell
>joe@debian-privesc:~$ cat /etc/passwd
>
># ========== Expected Result ==========
>root:x:0:0:root:/root:/bin/bash
>daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
>bin:x:2:2:bin:/bin:/usr/sbin/nologin
>sys:x:3:3:sys:/dev:/usr/sbin/nologin
>...
>www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
>...
>dnsmasq:x:106:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
>usbmux:x:107:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
>rtkit:x:108:114:RealtimeKit,,,:/proc:/usr/sbin/nologin
>sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
>...
>Debian-gdm:x:117:124:Gnome Display Manager:/var/lib/gdm3:/bin/false
>joe:x:1000:1000:joe,,,:/home/joe:/bin/bash
>systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin
>eve:x:1001:1001:,,,:/home/eve:/bin/bash
># =====================================
>```

Getting information about the hostname
>``` shell
>joe@debian-privesc:~$ hostname
>
># ========== Expected Result ==========
>debian-privesc
># =====================================
>```

Getting the version of the running operating system and architecture
>``` shell
>joe@debian-privesc:~$ cat /etc/issue
>
># ========== Expected Result ==========
>Debian GNU/Linux 10 \n \l
># =====================================
>
>joe@debian-privesc:~$ cat /etc/os-release
>
># ========== Expected Result ==========
>PRETTY_NAME="Debian GNU/Linux 10 (buster)"
>NAME="Debian GNU/Linux"
>VERSION_ID="10"
>VERSION="10 (buster)"
>VERSION_CODENAME=buster
>ID=debian
>HOME_URL="https://www.debian.org/"
>SUPPORT_URL="https://www.debian.org/support"
>BUG_REPORT_URL="https://bugs.debian.org/"
># =====================================
>
>joe@debian-privesc:~$ uname -a
>
># ========== Expected Result ==========
>Linux debian-privesc 4.19.0-21-amd64 #1 SMP Debian 4.19.249-2 (2022-06-30)
>x86_64 GNU/Linux
># =====================================
>```

Getting a list of running processes on Linux
>``` shell
>joe@debian-privesc:~$ ps aux
>
># ========== Expected Result ==========
>USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
>root         1  0.0  0.4 169592 10176 ?        Ss   Aug16   0:02 /sbin/init
>...
>colord     752  0.0  0.6 246984 12424 ?        Ssl  Aug16   0:00 /usr/lib/colord/colord
>Debian-+   753  0.0  0.2 157188  5248 ?        Sl   Aug16   0:00 /usr/lib/dconf/dconf-service
>root       477  0.0  0.5 179064 11060 ?        Ssl  Aug16   0:00 /usr/sbin/cups-browsed
>root       479  0.0  0.4 236048  9152 ?        Ssl  Aug16   0:00 /usr/lib/policykit-1/polkitd --no-debug
>root       486  0.0  1.0 123768 22104 ?        Ssl  Aug16   0:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
>root       510  0.0  0.3  13812  7288 ?        Ss   Aug16   0:00 /usr/sbin/sshd -D
>root       512  0.0  0.3 241852  8080 ?        Ssl  Aug16   0:00 /usr/sbin/gdm3
>root       519  0.0  0.4 166764  8308 ?        Sl   Aug16   0:00 gdm-session-worker [pam/gdm-launch-environment]
>root       530  0.0  0.2  11164  4448 ?        Ss   Aug16   0:03 /usr/sbin/apache2 -k start
>root      1545  0.0  0.0      0     0 ?        I    Aug16   0:00 [kworker/1:1-events]
>root      1653  0.0  0.3  14648  7712 ?        Ss   01:03   0:00 sshd: joe [priv]
>root      1656  0.0  0.0      0     0 ?        I    01:03   0:00 [kworker/1:2-events_power_efficient]
>joe       1657  0.0  0.4  21160  8960 ?        Ss   01:03   0:00 /lib/systemd/systemd --user
>joe       1658  0.0  0.1 170892  2532 ?        S    01:03   0:00 (sd-pam)
>joe       1672  0.0  0.2  14932  5064 ?        S    01:03   0:00 sshd: joe@pts/0
>joe       1673  0.0  0.2   8224  5020 pts/0    Ss   01:03   0:00 -bash
>root      1727  0.0  0.0      0     0 ?        I    03:00   0:00 [kworker/0:0-ata_sff]
>root      1728  0.0  0.0      0     0 ?        I    03:06   0:00 [kworker/0:2-ata_sff]
>joe       1730  0.0  0.1  10600  3028 pts/0    R+   03:10   0:00 ps axu
># =====================================
>```

Listing the full TCP/IP configuration on all available adapters on Linux
>``` shell
>joe@debian-privesc:~$ ip a
>
># ========== Expected Result ==========
>1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
>    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
>    inet 127.0.0.1/8 scope host lo
>       valid_lft forever preferred_lft forever
>    inet6 ::1/128 scope host
>       valid_lft forever preferred_lft forever
>2: ens192: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
>    link/ether 00:50:56:8a:b9:fc brd ff:ff:ff:ff:ff:ff
>    inet 192.168.50.214/24 brd 192.168.50.255 scope global ens192
>       valid_lft forever preferred_lft forever
>    inet6 fe80::250:56ff:fe8a:b9fc/64 scope link
>       valid_lft forever preferred_lft forever
>3: ens224: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
>    link/ether 00:50:56:8a:72:64 brd ff:ff:ff:ff:ff:ff
>    inet 172.16.60.214/24 brd 172.16.60.255 scope global ens224
>       valid_lft forever preferred_lft forever
>    inet6 fe80::250:56ff:fe8a:7264/64 scope link
>       valid_lft forever preferred_lft forever
># =====================================
>```

Printing the routes on Linux
>``` shell
>joe@debian-privesc:~$ routel
>
># ========== Expected Result ==========
>         target            gateway          source    proto    scope    dev tbl
>/usr/bin/routel: 48: shift: can't shift that many
>        default     192.168.50.254                   static          ens192
>    172.16.60.0 24                   172.16.60.214   kernel     link ens224
>   192.168.50.0 24                  192.168.50.214   kernel     link ens192
>      127.0.0.0          broadcast       127.0.0.1   kernel     link     lo local
>      127.0.0.0 8            local       127.0.0.1   kernel     host     lo local
>      127.0.0.1              local       127.0.0.1   kernel     host     lo local
>127.255.255.255          broadcast       127.0.0.1   kernel     link     lo local
>    172.16.60.0          broadcast   172.16.60.214   kernel     link ens224 local
>  172.16.60.214              local   172.16.60.214   kernel     host ens224 local
>  172.16.60.255          broadcast   172.16.60.214   kernel     link ens224 local
>   192.168.50.0          broadcast  192.168.50.214   kernel     link ens192 local
> 192.168.50.214              local  192.168.50.214   kernel     host ens192 local
> 192.168.50.255          broadcast  192.168.50.214   kernel     link ens192 local
>            ::1                                      kernel              lo
>         fe80:: 64                                   kernel          ens224
>         fe80:: 64                                   kernel          ens192
>            ::1              local                   kernel              lo local
>fe80::250:56ff:fe8a:7264              local                   kernel          ens224 local
>fe80::250:56ff:fe8a:b9fc              local                   kernel          ens192 local
># =====================================
>```

Listing all active network connections on Linux
>``` shell
>joe@debian-privesc:~$ ss -anp
>
># ========== Expected Result ==========
>Netid      State       Recv-Q      Send-Q                                        Local Address:Port                     Peer Address:Port
>nl         UNCONN      0           0                                                         0:461                                  *
>nl         UNCONN      0           0                                                         0:323                                  *
>nl         UNCONN      0           0                                                         0:457                                  *
>...
>udp        UNCONN      0           0                                                      [::]:47620                            [::]:*
>tcp        LISTEN      0           128                                                 0.0.0.0:22                            0.0.0.0:*
>tcp        LISTEN      0           5                                                 127.0.0.1:631                           0.0.0.0:*
>tcp        ESTAB       0           36                                           192.168.50.214:22                      192.168.118.2:32890
>tcp        LISTEN      0           128                                                       *:80                                  *:*
>tcp        LISTEN      0           128                                                    [::]:22                               [::]:*
>tcp        LISTEN      0           5                                                     [::1]:631                              [::]:*
># =====================================
>```

Inspecting custom IP tables
>``` shell
>joe@debian-privesc:~$ cat /etc/iptables/rules.v4
>
># ========== Expected Result ==========
># Generated by xtables-save v1.8.2 on Thu Aug 18 12:53:22 2022
>*filter
>:INPUT ACCEPT [0:0]
>:FORWARD ACCEPT [0:0]
>:OUTPUT ACCEPT [0:0]
>-A INPUT -p tcp -m tcp --dport 1999 -j ACCEPT
>COMMIT
># Completed on Thu Aug 18 12:53:22 2022
># =====================================
>```

Listing all cron jobs
>``` shell
>joe@debian-privesc:~$ ls -lah /etc/cron*
>
># ========== Expected Result ==========
>-rw-r--r-- 1 root root 1.1K Oct 11  2019 /etc/crontab
>
>/etc/cron.d:
>total 24K
>drwxr-xr-x   2 root root 4.0K Aug 16 04:25 .
>drwxr-xr-x 120 root root  12K Aug 18 12:37 ..
>-rw-r--r--   1 root root  102 Oct 11  2019 .placeholder
>-rw-r--r--   1 root root  285 May 19  2019 anacron
>
>/etc/cron.daily:
>total 60K
>drwxr-xr-x   2 root root 4.0K Aug 18 09:05 .
>drwxr-xr-x 120 root root  12K Aug 18 12:37 ..
>-rw-r--r--   1 root root  102 Oct 11  2019 .placeholder
>-rwxr-xr-x   1 root root  311 May 19  2019 0anacron
>-rwxr-xr-x   1 root root  539 Aug  8  2020 apache2
>-rwxr-xr-x   1 root root 1.5K Dec  7  2020 apt-compat
>-rwxr-xr-x   1 root root  355 Dec 29  2017 bsdmainutils
>-rwxr-xr-x   1 root root  384 Dec 31  2018 cracklib-runtime
>-rwxr-xr-x   1 root root 1.2K Apr 18  2019 dpkg
>-rwxr-xr-x   1 root root 2.2K Feb 10  2018 locate
>-rwxr-xr-x   1 root root  377 Aug 28  2018 logrotate
>-rwxr-xr-x   1 root root 1.1K Feb 10  2019 man-db
>-rwxr-xr-x   1 root root  249 Sep 27  2017 passwd
>
>/etc/cron.hourly:
>total 20K
>drwxr-xr-x   2 root root 4.0K Aug 16 04:17 .
>drwxr-xr-x 120 root root  12K Aug 18 12:37 ..
>-rw-r--r--   1 root root  102 Oct 11  2019 .placeholder
>
>/etc/cron.monthly:
>total 24K
>drwxr-xr-x   2 root root 4.0K Aug 16 04:25 .
>drwxr-xr-x 120 root root  12K Aug 18 12:37 ..
>-rw-r--r--   1 root root  102 Oct 11  2019 .placeholder
>-rwxr-xr-x   1 root root  313 May 19  2019 0anacron
>
>/etc/cron.weekly:
>total 28K
>drwxr-xr-x   2 root root 4.0K Aug 16 04:26 .
>drwxr-xr-x 120 root root  12K Aug 18 12:37 ..
>-rw-r--r--   1 root root  102 Oct 11  2019 .placeholder
>-rwxr-xr-x   1 root root  312 May 19  2019 0anacron
>-rwxr-xr-x   1 root root  813 Feb 10  2019 man-db
># =====================================
>```

Listing cron jobs for the current user
>``` shell
>joe@debian-privesc:~$ crontab -l
>
># ========== Expected Result ==========
># Edit this file to introduce tasks to be run by cron.
>#
># Each task to run has to be defined through a single line
># indicating with different fields when the task will be run
># and what command to run for the task
>#
># To define the time you can provide concrete values for
># minute (m), hour (h), day of month (dom), month (mon),
># and day of week (dow) or use '*' in these fields (for 'any').
>#
># Notice that tasks will be started based on the cron's system
># daemon's notion of time and timezones.
>#
># Output of the crontab jobs (including errors) is sent through
># email to the user the crontab file belongs to (unless redirected).
>#
># For example, you can run a backup of all your user accounts
># at 5 a.m every week with:
># 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
>#
># For more information see the manual pages of crontab(5) and cron(8)
>#
># m h  dom mon dow   command
># =====================================
>```

Listing cron jobs for the root user
>``` shell
>joe@debian-privesc:~$ sudo crontab -l
>
># ========== Expected Result ==========
>[sudo] password for joe:
># Edit this file to introduce tasks to be run by cron.
>...
># m h  dom mon dow   command
>
>* * * * * /bin/bash /home/joe/.scripts/user_backups.sh
># =====================================
>```

Listing all installed packages on a Debian Linux operating system
>``` shell
>joe@debian-privesc:~$ dpkg -l
>
># ========== Expected Result ==========
>Desired=Unknown/Install/Remove/Purge/Hold
>| Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
>|/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
>||/ Name                                  Version                                      Architecture Description
>+++-=====================================-============================================-============-===============================================================================
>ii  accountsservice                       0.6.45-2                                     amd64        query and manipulate user account information
>ii  acl                                   2.2.53-4                                     amd64        access control list - utilities
>ii  adduser                               3.118                                        all          add and remove users and groups
>ii  adwaita-icon-theme                    3.30.1-1                                     all          default icon theme of GNOME
>ii  aisleriot                             1:3.22.7-2                                   amd64        GNOME solitaire card game collection
>ii  alsa-utils                            1.1.8-2                                      amd64        Utilities for configuring and using ALSA
>ii  anacron                               2.3-28                                       amd64        cron-like program that doesn't go by time
>ii  analog                                2:6.0-22                                     amd64        web server log analyzer
>ii  apache2                               2.4.38-3+deb10u7                             amd64        Apache HTTP Server
>ii  apache2-bin                           2.4.38-3+deb10u7                             amd64        Apache HTTP Server (modules and other binary files)
>ii  apache2-data                          2.4.38-3+deb10u7                             all          Apache HTTP Server (common files)
>ii  apache2-doc                           2.4.38-3+deb10u7                             all          Apache HTTP Server (on-site documentation)
>ii  apache2-utils                         2.4.38-3+deb10u7                             amd64        Apache HTTP Server (utility programs for web servers)
>...
># =====================================
>```

Listing all world writable directories
>``` shell
>joe@debian-privesc:~$ find / -writable -type d 2>/dev/null
>
># ========== Expected Result ==========
>...
>/home/joe
>/home/joe/Videos
>/home/joe/Templates
>/home/joe/.local
>/home/joe/.local/share
>/home/joe/.local/share/sounds
>/home/joe/.local/share/evolution
>/home/joe/.local/share/evolution/tasks
>/home/joe/.local/share/evolution/tasks/system
>/home/joe/.local/share/evolution/tasks/trash
>/home/joe/.local/share/evolution/addressbook
>/home/joe/.local/share/evolution/addressbook/system
>/home/joe/.local/share/evolution/addressbook/system/photos
>/home/joe/.local/share/evolution/addressbook/trash
>/home/joe/.local/share/evolution/mail
>/home/joe/.local/share/evolution/mail/trash
>/home/joe/.local/share/evolution/memos
>/home/joe/.local/share/evolution/memos/system
>/home/joe/.local/share/evolution/memos/trash
>/home/joe/.local/share/evolution/calendar
>/home/joe/.local/share/evolution/calendar/system
>/home/joe/.local/share/evolution/calendar/trash
>/home/joe/.local/share/icc
>/home/joe/.local/share/gnome-shell
>/home/joe/.local/share/gnome-settings-daemon
>/home/joe/.local/share/keyrings
>/home/joe/.local/share/tracker
>/home/joe/.local/share/tracker/data
>/home/joe/.local/share/folks
>/home/joe/.local/share/gvfs-metadata
>/home/joe/.local/share/applications
>/home/joe/.local/share/nano
>/home/joe/Downloads
>/home/joe/.scripts
>/home/joe/Pictures
>/home/joe/.cache
>...
># =====================================
>```

Listing content of /etc/fstab and all mounted drives
>``` shell
>joe@debian-privesc:~$ cat /etc/fstab 
>
># ========== Expected Result ==========
>...
>UUID=60b4af9b-bc53-4213-909b-a2c5e090e261 /               ext4    errors=remount-ro 0       1
># swap was on /dev/sda5 during installation
>UUID=86dc11f3-4b41-4e06-b923-86e78eaddab7 none            swap    sw              0       0
>/dev/sr0        /media/cdrom0   udf,iso9660 user,noauto     0       0
># =====================================
>
>joe@debian-privesc:~$ mount
>
># ========== Expected Result ==========
>sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
>proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
>udev on /dev type devtmpfs (rw,nosuid,relatime,size=1001064k,nr_inodes=250266,mode=755)
>devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
>tmpfs on /run type tmpfs (rw,nosuid,noexec,relatime,size=204196k,mode=755)
>/dev/sda1 on / type ext4 (rw,relatime,errors=remount-ro)
>securityfs on /sys/kernel/security type securityfs (rw,nosuid,nodev,noexec,relatime)
>tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev)
>tmpfs on /run/lock type tmpfs (rw,nosuid,nodev,noexec,relatime,size=5120k)
>tmpfs on /sys/fs/cgroup type tmpfs (ro,nosuid,nodev,noexec,mode=755)
>cgroup2 on /sys/fs/cgroup/unified type cgroup2 (rw,nosuid,nodev,noexec,relatime,nsdelegate)
>cgroup on /sys/fs/cgroup/systemd type cgroup (rw,nosuid,nodev,noexec,relatime,xattr,name=systemd)
>pstore on /sys/fs/pstore type pstore (rw,nosuid,nodev,noexec,relatime)
>bpf on /sys/fs/bpf type bpf (rw,nosuid,nodev,noexec,relatime,mode=700)
>...
>systemd-1 on /proc/sys/fs/binfmt_misc type autofs (rw,relatime,fd=25,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=10550)
>mqueue on /dev/mqueue type mqueue (rw,relatime)
>debugfs on /sys/kernel/debug type debugfs (rw,relatime)
>hugetlbfs on /dev/hugepages type hugetlbfs (rw,relatime,pagesize=2M)
>tmpfs on /run/user/117 type tmpfs (rw,nosuid,nodev,relatime,size=204192k,mode=700,uid=117,gid=124)
>tmpfs on /run/user/1000 type tmpfs (rw,nosuid,nodev,relatime,size=204192k,mode=700,uid=1000,gid=1000)
>binfmt_misc on /proc/sys/fs/binfmt_misc type binfmt_misc (rw,relatime)
># =====================================
>```

Listing all available drives using lsblk
>``` shell
>joe@debian-privesc:~$ lsblk
>
># ========== Expected Result ==========
>NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
>sda      8:0    0   32G  0 disk
>|-sda1   8:1    0   31G  0 part /
>|-sda2   8:2    0    1K  0 part
>`-sda5   8:5    0  975M  0 part [SWAP]
>sr0     11:0    1 1024M  0 rom
># =====================================
>```

Listing loaded drivers
>``` shell
>joe@debian-privesc:~$ lsmod
>
># ========== Expected Result ==========
>Module                  Size  Used by
>binfmt_misc            20480  1
>rfkill                 28672  1
>sb_edac                24576  0
>crct10dif_pclmul       16384  0
>crc32_pclmul           16384  0
>ghash_clmulni_intel    16384  0
>vmw_balloon            20480  0
>...
>drm                   495616  5 vmwgfx,drm_kms_helper,ttm
>libata                270336  2 ata_piix,ata_generic
>vmw_pvscsi             28672  2
>scsi_mod              249856  5 vmw_pvscsi,sd_mod,libata,sg,sr_mod
>i2c_piix4              24576  0
>button                 20480  0
># =====================================
>```

Displaying additional information about a module
>``` shell
>joe@debian-privesc:~$ /sbin/modinfo libata
>
># ========== Expected Result ==========
>filename:       /lib/modules/4.19.0-21-amd64/kernel/drivers/ata/libata.ko
>version:        3.00
>license:        GPL
>description:    Library module for ATA devices
>author:         Jeff Garzik
>srcversion:     00E4F01BB3AA2AAF98137BF
>depends:        scsi_mod
>retpoline:      Y
>intree:         Y
>name:           libata
>vermagic:       4.19.0-21-amd64 SMP mod_unload modversions
>sig_id:         PKCS#7
>signer:         Debian Secure Boot CA
>sig_key:        4B:6E:F5:AB:CA:66:98:25:17:8E:05:2C:84:66:7C:CB:C0:53:1F:8C
>...
># =====================================
>```

Searching for SUID files
>``` shell
>joe@debian-privesc:~$ find / -perm -u=s -type f 2>/dev/null
>
># ========== Expected Result ==========
>/usr/bin/chsh
>/usr/bin/fusermount
>/usr/bin/chfn
>/usr/bin/passwd
>/usr/bin/sudo
>/usr/bin/pkexec
>/usr/bin/ntfs-3g
>/usr/bin/gpasswd
>/usr/bin/newgrp
>/usr/bin/bwrap
>/usr/bin/su
>/usr/bin/umount
>/usr/bin/mount
>/usr/lib/policykit-1/polkit-agent-helper-1
>/usr/lib/xorg/Xorg.wrap
>/usr/lib/eject/dmcrypt-get-device
>/usr/lib/openssh/ssh-keysign
>/usr/lib/spice-gtk/spice-client-glib-usb-acl-helper
>/usr/lib/dbus-1.0/dbus-daemon-launch-helper
>/usr/sbin/pppd
># =====================================
>```

Lab 1 - Connect to VM 1 with the provided credentials and replicate the manual enumeration techniques covered in this section. Inspect the target's OS information and its release details. What is the Linux distribution codename?
>``` shell
>
>```
>

Lab 2 - What crontab parameter is needed to list every cron job for the current user?
>``` shell
>
>```
>

Lab 3 - What is the inherited UID called that allows a given binary to be executed with root permissions even when launched by a lower-privileged user?
>``` shell
>
>```
>

Lab 4 - Connect to VM 1 with the provided credentials. The flag is inside one of the SUID binaries available on the system.
>``` shell
>
>```
>
