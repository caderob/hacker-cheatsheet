# Enumerating Jenkins

Jenkins in Browser
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Enumerating-Jenkins-1.png)

Initializing the Metasploit Database
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

Selecting Module and Viewing Options
>``` shell
>kali@kali:~$ msfconsole --quiet
>
>msf6 > use auxiliary/scanner/http/jenkins_enum
>
>msf6 auxiliary(scanner/http/jenkins_enum) > show options
>
># ========== Expected Result ==========
>Module options (auxiliary/scanner/http/jenkins_enum):                                                                       
>                                                                                                                            
>   Name       Current Setting  Required  Description                                                                        
>   ----       ---------------  --------  -----------                                                                        
>   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]                       
>   RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/  
>                                         using-metasploit.html                                                              
>   RPORT      80               yes       The target port (TCP)                                                              
>   SSL        false            no        Negotiate SSL/TLS for outgoing connections                                         
>   TARGETURI  /jenkins/        yes       The path to the Jenkins-CI application                                             
>   THREADS    1                yes       The number of concurrent threads (max one per host)                                
>   VHOST                       no        HTTP server virtual host                                                           
>                                                                                                                            
>
>View the full module info with the info, or info -d command.
># =====================================
>```

Configuring the Module
>``` shell
>msf6 auxiliary(scanner/http/jenkins_enum) > set RHOSTS automation.offseclab.io
>
># ========== Expected Result ==========
>RHOSTS => automation.offseclab.io
># =====================================
>
>msf6 auxiliary(scanner/http/jenkins_enum) > set TARGETURI /
>
># ========== Expected Result ==========
>TARGETURI => /
># =====================================
>```

Running the Module
>``` shell
>msf6 auxiliary(scanner/http/jenkins_enum) > run
>
># ========== Expected Result ==========
>[+] 198.18.53.73:80      - Jenkins Version 2.385
>[*] /script restricted (403)
>[*] /view/All/newJob restricted (403)
>[*] /asynchPeople/ restricted (403)
>[*] /systemInfo restricted (403)
>[*] Scanned 1 of 1 hosts (100% complete)
>[*] Auxiliary module execution completed
># =====================================
>```

Lab 1 - Run a directory busting attack and discover the hidden endpoint on jenkins. This endpoint will return a flag.
>``` shell
>
>```
>

Lab 2 - Which Metasploit module is used to enumerate Jenkins in the given section?
>C) jenkins_enum

Lab 3 - What was the purpose of setting the TARGETURI option to "/" in the Metasploit module?
>A) To specify the root directory of Jenkins
