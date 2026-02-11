# Setup and Work with MSF

Creating and initializing the Metasploit database
>``` shell
>kali@kali:~$ sudo msfdb init
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

Enabling the postgresql database service at boot time
>``` shell
>kali@kali:~$ sudo systemctl enable postgresql
>
># ========== Expected Result ==========
>Synchronizing state of postgresql.service with SysV service script with /lib/systemd/systemd-sysv-install.
>Executing: /lib/systemd/systemd-sysv-install enable postgresql
>Created symlink /etc/systemd/system/multi-user.target.wants/postgresql.service → /lib/systemd/system/postgresql.service.
># =====================================
>```

Starting the Metasploit Framework
>``` shell
>kali@kali:~$ sudo msfconsole
>
># ========== Expected Result ==========
>...                                                                              
>       =[ metasploit v6.2.20-dev                          ]
>+ -- --=[ 2251 exploits - 1187 auxiliary - 399 post       ]
>+ -- --=[ 951 payloads - 45 encoders - 11 nops            ]
>+ -- --=[ 9 evasion                                       ]
>
>Metasploit tip: Use help <command> to learn more 
>about any command
>Metasploit Documentation: https://docs.metasploit.com/
>
>msf6 >
># =====================================
>```

Confirming database connectivity
>``` shell
>msf6 > db_status
>
># ========== Expected Result ==========
>[*] Connected to msf. Connection type: postgresql.
># =====================================
>```

Help menu of MSF commands
>``` shell
>msf6 > help
>
># ========== Expected Result ==========
>Core Commands
>=============
>
>    Command       Description
>    -------       -----------
>    ?             Help menu
>    ...
>
>Module Commands
>===============
>
>    Command       Description
>    -------       -----------
>    ...
>    search        Searches module names and descriptions
>    show          Displays modules of a given type, or all modules
>    use           Interact with a module by name or search term/index
>
>    
>Job Commands
>============
>
>    Command       Description
>    -------       -----------
>    ...
>
>Resource Script Commands
>========================
>
>    Command       Description
>    -------       -----------
>    ...
>
>Database Backend Commands
>=========================
>
>    Command           Description
>    -------           -----------
>    ...
>    db_nmap           Executes nmap and records the output automatically
>    ...
>    hosts             List all hosts in the database
>    loot              List all loot in the database
>    notes             List all notes in the database
>    services          List all services in the database
>    vulns             List all vulnerabilities in the database
>    workspace         Switch between database workspaces
>
>Credentials Backend Commands
>============================
>
>    Command       Description
>    -------       -----------
>    creds         List all credentials in the database
>    
>Developer Commands
>==================
>
>    Command       Description
>    -------       -----------
>    ...
># =====================================
>```

Creating workspace pen200
>``` shell
>msf6 > workspace
>
># ========== Expected Result ==========
>* default
># =====================================
>
>msf6 > workspace -a pen200
>
># ========== Expected Result ==========
>[*] Added workspace: pen200
>[*] Workspace: pen200
># =====================================
>```

Using db_nmap to scan BRUTE2
>``` shell
>msf6 > db_nmap
>
># ========== Expected Result ==========
>[*] Usage: db_nmap [--save | [--help | -h]] [nmap options]
># =====================================
>
>msf6 > db_nmap -A 192.168.50.202
>
># ========== Expected Result ==========
>[*] Nmap: Starting Nmap 7.92 ( https://nmap.org ) at 2022-07-28 03:48 EDT
>[*] Nmap: Nmap scan report for 192.168.50.202
>[*] Nmap: Host is up (0.11s latency).
>[*] Nmap: Not shown: 993 closed tcp ports (reset)
>[*] Nmap: PORT     STATE SERVICE       VERSION
>[*] Nmap: 21/tcp   open  ftp?
>...
>[*] Nmap: 135/tcp  open  msrpc         Microsoft Windows RPC
>[*] Nmap: 139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
>[*] Nmap: 445/tcp  open  microsoft-ds?
>[*] Nmap: 3389/tcp open  ms-wbt-server Microsoft Terminal Services
>...
>[*] Nmap: 5357/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
>...
>[*] Nmap: 8000/tcp open  http          Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
>...
>[*] Nmap: Nmap done: 1 IP address (1 host up) scanned in 67.72 seconds
># =====================================
>```

Display all discovered hosts
>``` shell
>msf6 > hosts
>
># ========== Expected Result ==========
>Hosts
>=====
>
>address         mac  name  os_name       os_flavor  os_sp  purpose  info  comments
>-------         ---  ----  -------       ---------  -----  -------  ----  --------
>192.168.50.202             Windows 2016                    server
># =====================================
>```

Display all discovered services
>``` shell
>msf6 > services
>
># ========== Expected Result ==========
>Services
>========
>
>host            port  proto  name           state  info
>----            ----  -----  ----           -----  ----
>192.168.50.202  21    tcp    ftp            open
>192.168.50.202  135   tcp    msrpc          open   Microsoft Windows RPC
>192.168.50.202  139   tcp    netbios-ssn    open   Microsoft Windows netbios-ssn
>192.168.50.202  445   tcp    microsoft-ds   open
>192.168.50.202  3389  tcp    ms-wbt-server  open   Microsoft Terminal Services
>192.168.50.202  5357  tcp    http           open   Microsoft HTTPAPI httpd 2.0 SSDP/UPnP
>192.168.50.202  8000  tcp    http           open   Golang net/http server Go-IPFS json-rpc or InfluxDB API
># =====================================
>
>msf6 > services -p 8000
>
># ========== Expected Result ==========
>Services
>========
>
>host            port  proto  name  state  info
>----            ----  -----  ----  -----  ----
>192.168.50.202  8000  tcp    http  open   Golang net/http server Go-IPFS json-rpc or InfluxDB API
># =====================================
>```

Help flag for the show command
>``` shell
>msf6 > show -h
>
># ========== Expected Result ==========
>[*] Valid parameters for the "show" command are: all, encoders, nops, exploits, payloads, auxiliary, post, plugins, info, options
>[*] Additional module-specific parameters are: missing, advanced, evasion, targets, actions
># =====================================
>```

Lab 1 - What command creates and initializes the MSF database?
>msfdb init

Lab 2 - Start VM #1 and follow the steps from this section to perform a Nmap scan within Metasploit. What is the command to display all services from discovered hosts with port number 445?
>``` shell
>
>```
>

