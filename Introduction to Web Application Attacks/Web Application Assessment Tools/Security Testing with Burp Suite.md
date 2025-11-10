# Security Testing with Burp Suite

Starting Burp Suite
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-1.png)

Starting Burp Suite from a terminal shell
>``` shell
>kali@kali:~$ burpsuite
>```

Burp Suite JRE warning
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-2.png)

Burp Startup
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-3.png)

Burp Configuration
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-4.png)

Burp Suite User Interface
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-5.png)

Turning Off Intercept
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-6.png)

Proxy Listeners
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-7.png)

Firefox Proxy Configuration.
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-8.png)

Burp Suite HTTP History
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-9.png)

Inspecting the first HTTP request.
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-10.png)

Sending a Request to Repeater
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-11.png)

Burp Suite Repeater
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-12.png)

Burp Suite Repeater with Request and Response
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-13.png)

Setting up our /etc/hosts file for offsecwp
>``` shell
>kali@kali:~$ cat /etc/hosts
>
># ========== Expected Result ==========
>...
>192.168.50.16 offsecwp
># =====================================
>```

Simulating a failed WordPress login 
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-14.png)

Sending the POST request to Intruder
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-15.png)

Assigning the password value to the Intruder payload generator
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-16.png)

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

Pasting the first 10 rockyou entries
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-17.png)

Inspecting Intruder's attack results
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-18.png)

Logging to the WP admin console
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Security-Testing-with-Burp-Suite-19.png)

Lab 1 - We have been tasked to test the SMS Two-Factor authentication of a newly-developed web application. The SMS verification code is made by four digits. Which Burp tool is most suited to perform a brute force attack against the keyspace?
>intruder

Lab 2 - Repeat the steps we covered in this Learning Unit and enumerate the targets via Nmap, Wappalyzer and Gobuster by starting Walkthrough VM 1. When performing a file/directory brute force attack with Gobuster, what is the HTTP response code related to redirection?
>``` shell
>gobuster dir -u 192.168.179.16 -w /usr/share/wordlists/dirb/common.txt -t 5
>
># ========== Expected Result ==========
>...
>Error: the server returns a status code that matches the provided options for non existing urls. http://192.168.179.16/b39314b4-6278-4428-99a5-9ab047d4ad89 => 301 (Length: 0). To continue please exclude the status code or the length
># =====================================
>```
>301

Lab 3 - Start up the Walkthrough VM 1 and replicate the steps we covered in this Learning Unit for using Burp Suite. What is the default port Burp proxy is listening to?
>``` shell
># Open Burp Suite, navigate to the "Proxy" tab, then "Proxy settings"
>```
>8080

Lab 4 - We have a lot of mess on our hands, and the new DIRTBUSTER cleaning service is just what we need to help with the cleanup! You can visit their new site on the Module Exercise VM #1, but it is still under development. We wonder where they hid their admin portal. Once found the admin portal, log-in with the provided credentials to obtain the flag.
>``` shell
># Running Gobuster
>gobuster dir -u http://192.168.179.52 -w /usr/share/wordlists/dirb/common.txt -t 10
>
># ========== Expected Result ==========
>...
>/.htpasswd            (Status: 403) [Size: 279]
>/.htaccess            (Status: 403) [Size: 279]
>/.hta                 (Status: 403) [Size: 279]
>/index.html           (Status: 200) [Size: 439]
>/portal               (Status: 301) [Size: 317] [--> http://192.168.179.52/portal/]
>/server-status        (Status: 403) [Size: 279]
>...
># =====================================
>
># Navigate to http://192.168.179.52/portal/ and login with "admin" / "admin"
>```
>OS{4f854129a623d8fe5b9b2fa6fbf1f606}

Lab 5 - The DIRTBUSTER team finally changed their default credentials, but they are not very original. We complied at http://target_vm/passwords.txt of potential passwords from the DIRTBUSTER employee contact info - I am confident the password is in there somewhere. The username is still admin, and the new login portal is available at the web server root folder on the Module Exercise VM #2.
>``` shell
># Open Burp Suite, navigate to the "Target" tab, then "Open browser" under "Site map"
>
># In burp browser, navigate to http://192.168.179.52 and login with "admin" / "admin"
>
># In Burp Suite, right click on the Post request, and select "Send to Intruder"
>
># Navigate to the "Intruder" tab and highlight the password value (admin) from the request
>
># Select "Add §" beside "Positions"
>
># Navigate to http://192.168.179.52/passwords.txt and copy the list of passwords
>
># In Burp Suite, press the "Paste" button beside the "Payloads configuration" box, then "Start attack"
>
># Sort the results by "Length" to find the correct password
>
># ========== Expected Result ==========
>zeddemore
># =====================================
>
># Navigate to http://192.168.179.52 and login with "admin" / "zeddemore"
>```
>OS{b86def30b59642a1d7d8a4453f078f0a}
