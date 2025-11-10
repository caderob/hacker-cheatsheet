# Inspecting HTTP Response Headers and Sitemaps

Using the Network Tool to View Requests
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Inspecting-HTTP-Response-Headers-and-Sitemaps-21.png)

Viewing Response Headers in the Network Tool
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Inspecting-HTTP-Response-Headers-and-Sitemaps-22.png)

https://www.google.com/robots.txt
>``` shell
>kali@kali:~$ curl https://www.google.com/robots.txt
>
># ========== Expected Result ==========
>User-agent: *
>Disallow: /search
>Allow: /search/about
>Allow: /search/static
>Allow: /search/howsearchworks
>Disallow: /sdch
>Disallow: /groups
>Disallow: /index.html?
>Disallow: /?
>Allow: /?hl=
>...
># =====================================
>```
