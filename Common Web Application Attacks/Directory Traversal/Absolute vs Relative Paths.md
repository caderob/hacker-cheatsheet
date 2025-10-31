# Absolute vs Relative Paths

Display content of /etc/passwd with an absolute path
>``` shell
>kali@kali:~$ pwd
>
># ========== Expected Result ==========
>/home/kali
># =====================================
>
>kali@kali:~$ ls /
>
># ========== Expected Result ==========
>bin   home            lib32       media  root  sys  vmlinuz
>boot  initrd.img      lib64       mnt    run   tmp  vmlinuz.old
>dev   initrd.img.old  libx32      opt    sbin  usr
>etc   lib             lost+found  proc   srv   var
># =====================================
>
>kali@kali:~$ cat /etc/passwd
>
># ========== Expected Result ==========
>root:x:0:0:root:/root:/usr/bin/zsh
>daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
>bin:x:2:2:bin:/bin:/usr/sbin/nologin
>sys:x:3:3:sys:/dev:/usr/sbin/nologin
>...
>king-phisher:x:133:141::/var/lib/king-phisher:/usr/sbin/nologin
>kali:x:1000:1000:Kali,,,:/home/kali:/usr/bin/zsh
># =====================================
>```

Using ../ to get to the root file system
>``` shell
>kali@kali:~$ pwd
>
># ========== Expected Result ==========
>/home/kali
># =====================================
>
>kali@kali:~$ ls ../
>
># ========== Expected Result ==========
>kali
># =====================================
>
>kali@kali:~$ ls ../../
>
># ========== Expected Result ==========
>bin   home            lib32       media  root  sys  vmlinuz
>boot  initrd.img      lib64       mnt    run   tmp  vmlinuz.old
>dev   initrd.img.old  libx32      opt    sbin  usr
>etc   lib             lost+found  proc   srv   var
># =====================================
>```

Display contents of /etc/passwd with a relative path
>``` shell
>kali@kali:~$ ls ../../etc
>
># ========== Expected Result ==========
>adduser.conf            debian_version  hostname        logrotate.d     passwd 
>...
>logrotate.conf  pam.d           rmt          sudoers       zsh
># =====================================
>
>kali@kali:~$ cat ../../etc/passwd
>
># ========== Expected Result ==========
>root:x:0:0:root:/root:/usr/bin/zsh
>daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
>bin:x:2:2:bin:/bin:/usr/sbin/nologin
>sys:x:3:3:sys:/dev:/usr/sbin/nologin
>...
>king-phisher:x:133:141::/var/lib/king-phisher:/usr/sbin/nologin
>kali:x:1000:1000:Kali,,,:/home/kali:/usr/bin/zsh
># =====================================
>```

Adding more "../" to the relative path
>``` shell
>kali@kali:~$ cat ../../../../../../../../../../../etc/passwd
>
># ========== Expected Result ==========
>root:x:0:0:root:/root:/usr/bin/zsh
>daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
>bin:x:2:2:bin:/bin:/usr/sbin/nologin
>sys:x:3:3:sys:/dev:/usr/sbin/nologin
>...
>king-phisher:x:133:141::/var/lib/king-phisher:/usr/sbin/nologin
>kali:x:1000:1000:Kali,,,:/home/kali:/usr/bin/zsh
># =====================================
>```

Lab 1 - How many ../ do you need to go from the /var/log/ directory to the root file system (/)? Enter the number below.
>2

Lab 2 - Enter the command in combination with the relative path containing the minimum number of ../ sequences to display the contents of the /etc/passwd file when the current working directory of the terminal is /usr/share/webshells/.
>cat ../../../etc/passwd
