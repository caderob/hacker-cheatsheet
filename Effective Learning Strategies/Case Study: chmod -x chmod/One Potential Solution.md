# One Potential Solution

Copying a file with cp.
>``` shell
>kali@kali:~$ cp /usr/bin/ls chmodfix
>
>kali@kali:~$ ls -l chmodfix
>
># ========== Expected Result ==========
>-rwxr-xr-x 1 kali kali 147176 Jun  8 08:16 chmodfix
># =====================================
>```

Anything ls can do, chmodfix can do.
>``` shell
>kali@kali:~$ ./chmodfix -l chmodfix
>
># ========== Expected Result ==========
>-rwxr-xr-x 1 kali kali 147176 Jun  8 08:16 chmodfix
># =====================================
>```

Sending the contents of chmod to chmodfix.
>``` shell
>kali@kali:~$ ls -l chmodfix
>
># ========== Expected Result ==========
>-rwxr-xr-x 1 kali kali 147176 Jun  8 08:20 chmodfix
># =====================================
>
>kali@kali:~$ cat /usr/bin/chmod > chmodfix
>
>kali@kali:~$ ls -l chmodfix
>
># ========== Expected Result ==========
>-rwxr-xr-x 1 kali kali 64448 Jun  8 08:21 chmodfix
># =====================================
>```

Our fix worked!
>``` shell
>kali@kali:~$ ./chmodfix +x find_employee_names.py
>
>kali@kali:~$ ./find_employee_names.py
>
># ========== Expected Result ==========
>R. Jones
>R. Diggs
>G. Grice
>C. Smith
>C. Woods
>D. Coles
>J. Hunter
>L. Hawkins
>E. Turner
>D. Hill
># =====================================
>```

Another obstacle.
>``` shell
>kali@kali:~$ ./chmodfix +x /usr/bin/chmod
>
># ========== Expected Result ==========
>./chmodfix: changing permissions of '/usr/bin/chmod': Operation not permitted
># =====================================
>```

Yo dawg, I heard you like chmod, so I chmod +x your chmod.
>``` shell
>kali@kali:~$ sudo ./chmodfix +x /usr/bin/chmod
>
># ========== Expected Result ==========
>[sudo] password for kali: 
># =====================================
>```
