# Security Testing with Burp Suite

Setting up our /etc/hosts file for offsecwp
>``` shell
>kali@kali:~$ cat /etc/hosts
>
># ========== Expected Result ==========
>...
>192.168.50.16 offsecwp
># =====================================
>```


Setting up our /etc/hosts file for offsecwp
>``` shell
>kali@kali:~$ cat /usr/share/wordlists/rockyou.txt | head
>
># ========== Expected Result ==========
>123456
>12345
>123456789
>password
>iloveyou
>princess
>1234567
>rockyou
>12345678
>abc123
># =====================================
>```

Lab 1 - We have been tasked to test the SMS Two-Factor authentication of a newly-developed web application. The SMS verification code is made by four digits. Which Burp tool is most suited to perform a brute force attack against the keyspace?
>``` shell
>
>```
>intruder

Lab 2 - Repeat the steps we covered in this Learning Unit and enumerate the targets via Nmap, Wappalyzer and Gobuster by starting Walkthrough VM 1. When performing a file/directory brute force attack with Gobuster, what is the HTTP response code related to redirection?
>``` shell
>
>```
>301

Lab 3 - Start up the Walkthrough VM 1 and replicate the steps we covered in this Learning Unit for using Burp Suite. What is the default port Burp proxy is listening to?
>``` shell
>
>```
>8080

Lab 4 - We have a lot of mess on our hands, and the new DIRTBUSTER cleaning service is just what we need to help with the cleanup! You can visit their new site on the Module Exercise VM #1, but it is still under development. We wonder where they hid their admin portal. Once found the admin portal, log-in with the provided credentials to obtain the flag.
>``` shell
>
>```
>OS{4f854129a623d8fe5b9b2fa6fbf1f606}

Lab 5 - The DIRTBUSTER team finally changed their default credentials, but they are not very original. We complied at http://target_vm/passwords.txt of potential passwords from the DIRTBUSTER employee contact info - I am confident the password is in there somewhere. The username is still admin, and the new login portal is available at the web server root folder on the Module Exercise VM #2.
>``` shell
>
>```
>OS{b86def30b59642a1d7d8a4453f078f0a}
