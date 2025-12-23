# Mutating Wordlists

Displaying the first 10 passwords of rockyou.txt
>``` shell
>kali@kali:~$ head /usr/share/wordlists/rockyou.txt 
>
># ========== Expected Result ==========
>123456
>12345
>123456789
>password
>iloveyou
>princess
>1234567
>rockyou
>12345678
>abc123
># =====================================
>```

Contents and location of demo.txt
>``` shell
>kali@kali:~$ mkdir passwordattacks
>
>kali@kali:~$ cd passwordattacks
>
>kali@kali:~/passwordattacks$ head /usr/share/wordlists/rockyou.txt > demo.txt
>
>kali@kali:~/passwordattacks$ sed -i '/^1/d' demo.txt 
>
>kali@kali:~/passwordattacks$ cat demo.txt
>
># ========== Expected Result ==========
>password
>iloveyou
>princess
>rockyou
>abc123
># =====================================
>```

Rule function to add a "1" to all passwords
>``` shell
>kali@kali:~/passwordattacks$ echo \$1 > demo.rule
>```

Using Hashcat in debugging mode to display all mutated passwords
>``` shell
>kali@kali:~/passwordattacks$ hashcat -r demo.rule --stdout demo.txt
>
># ========== Expected Result ==========
>password1
>iloveyou1
>princess1
>rockyou1
>abc1231
># =====================================
>```

Using two rule functions separated by space and line
>``` shell
>kali@kali:~/passwordattacks$ cat demo1.rule
>
># ========== Expected Result ==========
>$1 c
># =====================================
>
>kali@kali:~/passwordattacks$ hashcat -r demo1.rule --stdout demo.txt
>
># ========== Expected Result ==========
>Password1
>Iloveyou1
>Princess1
>Rockyou1
>Abc1231
># =====================================
>
>kali@kali:~/passwordattacks$ cat demo2.rule
>
># ========== Expected Result ==========
>$1
>c
># =====================================
>
>kali@kali:~/passwordattacks$ hashcat -r demo2.rule --stdout demo.txt
>
># ========== Expected Result ==========
>password1
>Password
>iloveyou1
>Iloveyou
>princess1
>Princess
>...
># =====================================
>```

Adding the rule function to the beginning and end of our current rule
>``` shell
>kali@kali:~/passwordattacks$ cat demo1.rule
>
># ========== Expected Result ==========
>$1 c $!
># =====================================
>
>kali@kali:~/passwordattacks$ hashcat -r demo1.rule --stdout demo.txt
>
># ========== Expected Result ==========
>Password1!
>Iloveyou1!
>Princess1!
>Rockyou1!
>Abc1231!
># =====================================
>
>kali@kali:~/passwordattacks$ cat demo2.rule
>
># ========== Expected Result ==========
>$! $1 c
># =====================================
>
>kali@kali:~/passwordattacks$ hashcat -r demo2.rule --stdout demo.txt
>
># ========== Expected Result ==========
>Password!1
>Iloveyou!1
>Princess!1
>Rockyou!1
>Abc123!1
># =====================================
>```

MD5 Hash and rule file
>``` shell
>kali@kali:~/passwordattacks$ cat crackme.txt
>
># ========== Expected Result ==========
>f621b6c9eab51a3e2f4e167fee4c6860
># =====================================
>
>kali@kali:~/passwordattacks$ cat demo3.rule
>
># ========== Expected Result ==========
>$1 c $!
>$2 c $!
>$1 $2 $3 c $!
># =====================================
>```

Cracking a MD5 Hash with Hashcat and a mutated rockyou.txt wordlist
>``` shell
>kali@kali:~/passwordattacks$ hashcat -m 0 crackme.txt /usr/share/wordlists/rockyou.txt -r demo3.rule --force
>
># ========== Expected Result ==========
>hashcat (v6.2.5) starting
>...
>Dictionary cache hit:
>* Filename..: /usr/share/wordlists/rockyou.txt
>* Passwords.: 14344385
>* Bytes.....: 139921507
>* Keyspace..: 43033155
>
>f621b6c9eab51a3e2f4e167fee4c6860:Computer123!            
>                                                          
>Session..........: hashcat
>Status...........: Cracked
>Hash.Mode........: 0 (MD5)
>Hash.Target......: f621b6c9eab51a3e2f4e167fee4c6860
>Time.Started.....: Tue May 24 14:34:54 2022, (0 secs)
>Time.Estimated...: Tue May 24 14:34:54 2022, (0 secs)
>Kernel.Feature...: Pure Kernel
>Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
>Guess.Mod........: Rules (demo3.rule)
>Guess.Queue......: 1/1 (100.00%)
>Speed.#1.........:  3144.1 kH/s (0.28ms) @ Accel:256 Loops:3 Thr:1 Vec:8
>Recovered........: 1/1 (100.00%) Digests
>...
># =====================================
>```

Listing of Hashcat's rule files
>``` shell
>kali@kali:~/passwordattacks$ ls -la /usr/share/hashcat/rules/
>
># ========== Expected Result ==========
>total 2588
>-rw-r--r-- 1 root root    933 Dec 23 08:53 best66.rule
>-rw-r--r-- 1 root root    666 Dec 23 08:53 combinator.rule
>-rw-r--r-- 1 root root 200188 Dec 23 08:53 d3ad0ne.rule
>-rw-r--r-- 1 root root 788063 Dec 23 08:53 dive.rule
>-rw-r--r-- 1 root root 483425 Dec 23 08:53 generated2.rule
>-rw-r--r-- 1 root root  78068 Dec 23 08:53 generated.rule
>drwxr-xr-x 2 root root   4096 Feb 11 01:58 hybrid
>-rw-r--r-- 1 root root 309439 Dec 23 08:53 Incisive-leetspeak.rule
>-rw-r--r-- 1 root root  35280 Dec 23 08:53 InsidePro-HashManager.rule
>-rw-r--r-- 1 root root  19478 Dec 23 08:53 InsidePro-PasswordsPro.rule
>-rw-r--r-- 1 root root    298 Dec 23 08:53 leetspeak.rule
>-rw-r--r-- 1 root root   1280 Dec 23 08:53 oscommerce.rule
>-rw-r--r-- 1 root root 301161 Dec 23 08:53 rockyou-30000.rule
>-rw-r--r-- 1 root root   1563 Dec 23 08:53 specific.rule
>-rw-r--r-- 1 root root  64068 Dec 23 08:53 T0XlC-insert_00-99_1950-2050_toprules_0_F.rule
>...
># =====================================
>```

Lab 1 - You extracted the MD5 hash "056df33e47082c77148dba529212d50a" from a target system. Create a rule to add "1@3$5" to each password of the rockyou.txt wordlist and crack the hash.
>``` shell
># Save the target MD5 hash to a file
>kali@kali:~$ echo "056df33e47082c77148dba529212d50a" > hash.txt
>
># Create a Hashcat rule to append 1@3$5
>kali@kali:~$ cat << 'EOF' > demo.rule
>$1$@$3$$$5
>EOF
>
># Verify the rule output
>kali@kali:~$ hashcat --stdout -r demo.rule /usr/share/wordlists/rockyou.txt | head
>
># Run Hashcat with the custom rule
>kali@kali:~$ hashcat -m 0 -a 0 hash.txt /usr/share/wordlists/rockyou.txt -r demo.rule
>
># Display the cracked password
>kali@kali:~$ hashcat -m 0 hash.txt --show
>
># ========== Expected Result ==========
>056df33e47082c77148dba529212d50a:courtney1@3$
># =====================================
>```
>courtney1@3$

Lab 2 - You extracted the MD5 hash "19adc0e8921336d08502c039dc297ff8" from a target system. Create a rule which makes all letters upper case and duplicates the passwords contained in rockyou.txt and crack the hash.
>``` shell
>
>```
>
