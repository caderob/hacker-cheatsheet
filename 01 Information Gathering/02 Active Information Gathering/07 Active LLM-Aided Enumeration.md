# Active LLM-Aided Enumeration

LLM's prompt to generate a DNS subdomain wordlist
>``` shell
>Using public data from MegacorpOne's website and any information that can be inferred about its organizational structure, products, or services, generate a comprehensive list of potential subdomain names.
>	•	Incorporate common patterns used for subdomains, such as:
>	•	Infrastructure-related terms (e.g., "api", "dev", "test", "staging").
>	•	Service-specific terms (e.g., "mail", "auth", "cdn", "status").
>	•	Departmental or functional terms (e.g., "hr", "sales", "support").
>	•	Regional or country-specific terms (e.g., "us", "eu", "asia").
>	•	Factor in industry norms and frequently used terms relevant to MegacorpOne's sector.
>
>Finally, compile the generated terms into a structured wordlist of 1000  words, optimized for subdomain brute-forcing against megacorpone.com
>
>Ensure the output is in a clean, lowercase format with no duplicates, no bulletpoints and ready to be copied and pasted.
>Make sure the list contains 1000 unique entries.
>
># ========== Expected Result ==========
>I have generated a structured 1000-word subdomain wordlist optimized for brute-forcing against megacorpone.com. You can download the file for use directly:
>
>Download Subdomain Wordlist
># =====================================
>```

Installing gobuster on Kali
>``` shell
>kali@kali:~$ sudo apt update
>kali@kali:~$ sudo apt install gobuster
>```

Running gobuster DNS subdomain enumeration with a wordlist
>``` shell
>kali@kali:~$ gobuster dns -d megacorpone.com -w wordlist.txt -t 10
>
># ========== Expected Result ==========
>===============================================================
>Gobuster v3.6
>by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
>===============================================================
>[+] Domain:     megacorpone.com
>[+] Threads:    10
>[+] Timeout:    1s
>[+] Wordlist:   wordlist
>===============================================================
>Starting gobuster in DNS enumeration mode
>===============================================================
>[INFO] [-] Unable to validate base domain: megacorpone.com (lookup megacorpone.com on 8.8.8.8:53: no such host)
>Found: admin.megacorpone.com
>
>Found: router.megacorpone.com
>
>Found: mail.megacorpone.com
>
>Found: support.megacorpone.com
>
>Found: test.megacorpone.com
>...
># =====================================
>```
