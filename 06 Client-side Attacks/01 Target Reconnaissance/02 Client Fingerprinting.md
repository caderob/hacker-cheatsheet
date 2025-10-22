# Client Fingerprinting

>Create a link in Canarytokens to obtain operating system and browser information from a target
>``` shell
>https://canarytokens.org/generate
>```

Lab 1 - Is there a difference in the results regarding enabled or disabled AdBlocker?
>Yes (false)

Lab 2 - Find a hidden PDF file and extract the flag in the metadata
>``` shell
>kali@kali:~$ gobuster dir -u http://192.168.103.197 -w /usr/share/wordlists/dirb/common.txt -x pdf -t 5
>
>># ========== Expected Result ==========
>...
>/index.html           (Status: 200) [Size: 5443]
>/info.pdf             (Status: 200) [Size: 309737]
>/old.pdf              (Status: 200) [Size: 462554]
>...
># =====================================
>
>Download info.pdf (http://192.168.103.197/info.pdf)
>
>kali@kali:~$ cd Downloads 
>
>kali@kali:~/Downloads$ exiftool -a -u info.pdf 
>
># ========== Expected Result ==========
>...
>Description                     : OS{9d27aa56204924e6fc0a9bb13934282a}
># =====================================
>```
>OS{9d27aa56204924e6fc0a9bb13934282a}
