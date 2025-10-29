# Inspecting HTTP Response Headers and Sitemaps

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
