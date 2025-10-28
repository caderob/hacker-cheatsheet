# What is Executable Permission?

Checking file permissions
>``` shell
>kali@kali:~$ touch newfilename.txt
>
>kali@kali:~$ ls -l newfilename.txt
>
># ========== Expected Result ==========
>-rw-r--r-- 1 kali kali 0 Jun  6 12:31 newfilename.txt
># =====================================
>```

The first attempt at running our script.
>``` shell
>kali@kali:~$ ./find_employee_names.py
>
># ========== Expected Result ==========
>zsh: permission denied: ./find_employee_names.py
># =====================================
>
>kali@kali:~$ ls -l newfilename.txt
>
># ========== Expected Result ==========
>-rw-r--r-- 1 kali kali 206 Jun  7 12:31 find_employee_names.py
># =====================================
>```

Second attempt after chmod.
>``` shell
>kali@kali:~$ chmod +x find_employee_names.py
>
>kali@kali:~$ ls -l find_employee_names.py
>
># ========== Expected Result ==========
>-rwxr-xr-x 1 kali kali 206 Jun  7 12:31 find_employee_names.py
># =====================================
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

Putting things back the way they were
>``` shell
>kali@kali:~$ chmod -x find_employee_names.py
>
>kali@kali:~$ ./find_employee_names.py
>
># ========== Expected Result ==========
>zsh: permission denied: ./find_employee_names.py
># =====================================
>```
