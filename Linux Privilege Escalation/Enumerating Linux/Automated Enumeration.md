# Automated Enumeration

Running unix_privesc_check
>``` shell
>kali@kali:~$ unix-privesc-check
>
># ========== Expected Result ==========
>unix-privesc-check v1.4 ( http://pentestmonkey.net/tools/unix-privesc-check )
>
>Usage: unix-privesc-check { standard | detailed }
>
>"standard" mode: Speed-optimised check of lots of security settings.
>
>"detailed" mode: Same as standard mode, but also checks perms of open file
>                 handles and called files (e.g. parsed from shell scripts,
>                 linked .so files).  This mode is slow and prone to false 
>                 positives but might help you find more subtle flaws in 3rd
>                 party programs.
>
>This script checks file permissions and other settings that could allow
>local users to escalate privileges.
>...
># =====================================
>```

Running unix_privesc_check
>``` shell
>joe@debian-privesc:~$ ./unix-privesc-check standard > output.txt
>```

unix_privesc_check writable configuration files
>``` shell
>Checking for writable config files
>############################################
>    Checking if anyone except root can change /etc/passwd
>WARNING: /etc/passwd is a critical config file. World write is set for /etc/passwd
>    Checking if anyone except root can change /etc/group
>    Checking if anyone except root can change /etc/fstab
>    Checking if anyone except root can change /etc/profile
>    Checking if anyone except root can change /etc/sudoers
>    Checking if anyone except root can change /etc/shadow
>```

Lab 1 - Connect to VM 1 with the provided credentials and run unix-privesc-check in standard mode. The flag is inside a file that normally should not be world-writable.
>``` shell
>
>```
>
