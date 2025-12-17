# Hacker Cheatsheet

This repository serves as a comprehensive compilation of notes and commands assembled during preparation for the Offensive Security Certified Professional (OSCP) certification, specifically the PEN-200 course taken in 2025. 

The purpose of this cheatsheet is to provide a structured and concise reference to aid in the understanding and execution of various penetration testing techniques covered in the OSCP curriculum.

## Contents

>- [**Penetration Testing with Kali Linux: General Course Information**](https://github.com/caderob/hacker-cheatsheet/tree/main/Penetration%20Testing%20with%20Kali%20Linux%3A%20General%20Course%20Information)
>   - [Getting Started with PWK](https://github.com/caderob/hacker-cheatsheet/tree/main/Penetration%20Testing%20with%20Kali%20Linux%3A%20General%20Course%20Information/Getting%20Started%20with%20PWK)
>     - [PWK Course Materials](https://github.com/caderob/hacker-cheatsheet/blob/main/Penetration%20Testing%20with%20Kali%20Linux%3A%20General%20Course%20Information/Getting%20Started%20with%20PWK/PWK%20Course%20Materials.md)
>     - [Connecting to the PWK Lab](https://github.com/caderob/hacker-cheatsheet/blob/main/Penetration%20Testing%20with%20Kali%20Linux%3A%20General%20Course%20Information/Getting%20Started%20with%20PWK/Connecting%20to%20the%20PWK%20Lab.md)
>- [**Introduction To Cybersecurity**](https://github.com/caderob/hacker-cheatsheet/tree/main/Introduction%20To%20Cybersecurity)
>   - [The Practice of Cybersecurity](https://github.com/caderob/hacker-cheatsheet/tree/main/Introduction%20To%20Cybersecurity/The%20Practice%20of%20Cybersecurity)
>     - [On Emulating the Minds of our Opponents](https://github.com/caderob/hacker-cheatsheet/blob/main/Introduction%20To%20Cybersecurity/The%20Practice%20of%20Cybersecurity/On%20Emulating%20the%20Minds%20of%20our%20Opponents.md)
>   - [Threats and Threat Actors](https://github.com/caderob/hacker-cheatsheet/tree/main/Introduction%20To%20Cybersecurity/Threats%20and%20Threat%20Actors)
>     - [Recent Cybersecurity Breaches](https://github.com/caderob/hacker-cheatsheet/blob/main/Introduction%20To%20Cybersecurity/Threats%20and%20Threat%20Actors/Recent%20Cybersecurity%20Breaches.md) 
>   - [The CIA Triad](https://github.com/caderob/hacker-cheatsheet/tree/main/Introduction%20To%20Cybersecurity/The%20CIA%20Triad)
>     - [Balancing the Triad with Organizational Objectives](https://github.com/caderob/hacker-cheatsheet/blob/main/Introduction%20To%20Cybersecurity/The%20CIA%20Triad/Balancing%20the%20Triad%20with%20Organizational%20Objectives.md)
>   - [Security Principles, Controls, and Strategies](https://github.com/caderob/hacker-cheatsheet/tree/main/Introduction%20To%20Cybersecurity/Security%20Principles%2C%20Controls%2C%20and%20Strategies)
>     - [Logging and Chaos Testing](https://github.com/caderob/hacker-cheatsheet/blob/main/Introduction%20To%20Cybersecurity/Security%20Principles%2C%20Controls%2C%20and%20Strategies/Logging%20and%20Chaos%20Testing.md) 
>   - [Cybersecurity Laws, Regulations, Standards, and Frameworks](https://github.com/caderob/hacker-cheatsheet/tree/main/Introduction%20To%20Cybersecurity/Cybersecurity%20Laws%2C%20Regulations%2C%20Standards%2C%20and%20Frameworks)
>     - [Anatomy of Cyber](https://github.com/caderob/hacker-cheatsheet/blob/main/Introduction%20To%20Cybersecurity/Cybersecurity%20Laws%2C%20Regulations%2C%20Standards%2C%20and%20Frameworks/Anatomy%20of%20Cyber.md)
>   - [Career Opportunities in Cybersecurity](https://github.com/caderob/hacker-cheatsheet/tree/main/Introduction%20To%20Cybersecurity/Career%20Opportunities%20in%20Cybersecurity)
>     - [Additional Roles](https://github.com/caderob/hacker-cheatsheet/blob/main/Introduction%20To%20Cybersecurity/Career%20Opportunities%20in%20Cybersecurity/Additional%20Roles.md)
>- [**Effective Learning Strategies**](https://github.com/caderob/hacker-cheatsheet/tree/main/Effective%20Learning%20Strategies)
>   - [Learning Theory](https://github.com/caderob/hacker-cheatsheet/tree/main/Effective%20Learning%20Strategies/Learning%20Theory)
>     - [The Forgetting Curve and Cognitive Load](https://github.com/caderob/hacker-cheatsheet/blob/main/Effective%20Learning%20Strategies/Learning%20Theory/The%20Forgetting%20Curve%20and%20Cognitive%20Load.md)
>   - [Unique Challenges to Learning Technical Skills](https://github.com/caderob/hacker-cheatsheet/tree/main/Effective%20Learning%20Strategies/Unique%20Challenges%20to%20Learning%20Technical%20Skills)
>     - [The Challenges of Remote and Asynchronous Learning](https://github.com/caderob/hacker-cheatsheet/blob/main/Effective%20Learning%20Strategies/Unique%20Challenges%20to%20Learning%20Technical%20Skills/The%20Challenges%20of%20Remote%20and%20Asynchronous%20Learning.md)
>   - [OffSec Training Methodology](https://github.com/caderob/hacker-cheatsheet/tree/main/Effective%20Learning%20Strategies/OffSec%20Training%20Methodology)
>     - [The Demonstration Method](https://github.com/caderob/hacker-cheatsheet/blob/main/Effective%20Learning%20Strategies/OffSec%20Training%20Methodology/The%20Demonstration%20Method.md)
>     - [Contextual Learning and Interleaving](https://github.com/caderob/hacker-cheatsheet/blob/main/Effective%20Learning%20Strategies/OffSec%20Training%20Methodology/Contextual%20Learning%20and%20Interleaving.md)
>   - [Case Study: chmod -x chmod](https://github.com/caderob/hacker-cheatsheet/tree/main/Effective%20Learning%20Strategies/Case%20Study%3A%20chmod%20-x%20chmod)
>     - [What is Executable Permission?](https://github.com/caderob/hacker-cheatsheet/blob/main/Effective%20Learning%20Strategies/Case%20Study%3A%20chmod%20-x%20chmod/What%20is%20Executable%20Permission.md)
>     - [Going Deeper: Encountering a Strange Problem](https://github.com/caderob/hacker-cheatsheet/blob/main/Effective%20Learning%20Strategies/Case%20Study%3A%20chmod%20-x%20chmod/Going%20Deeper%3A%20Encountering%20a%20Strange%20Problem.md)
>     - [One Potential Solution](https://github.com/caderob/hacker-cheatsheet/blob/main/Effective%20Learning%20Strategies/Case%20Study%3A%20chmod%20-x%20chmod/One%20Potential%20Solution.md)
>     - [Analyzing this Approach](https://github.com/caderob/hacker-cheatsheet/blob/main/Effective%20Learning%20Strategies/Case%20Study%3A%20chmod%20-x%20chmod/Analyzing%20this%20Approach.md)
>- [**Report Writing for Penetration Testers**](https://github.com/caderob/hacker-cheatsheet/tree/main/Report%20Writing%20for%20Penetration%20Testers)
>   - [Understanding Note-Taking](https://github.com/caderob/hacker-cheatsheet/tree/main/Report%20Writing%20for%20Penetration%20Testers/Understanding%20Note-Taking)
>     - [The General Structure of Penetration Testing Notes.md](https://github.com/caderob/hacker-cheatsheet/blob/main/Report%20Writing%20for%20Penetration%20Testers/Understanding%20Note-Taking/The%20General%20Structure%20of%20Penetration%20Testing%20Notes.md)
>     - [Choosing the Right Note-Taking Tool](https://github.com/caderob/hacker-cheatsheet/blob/main/Report%20Writing%20for%20Penetration%20Testers/Understanding%20Note-Taking/Choosing%20the%20Right%20Note-Taking%20Tool.md)
>     - [Taking Screenshots](https://github.com/caderob/hacker-cheatsheet/blob/main/Report%20Writing%20for%20Penetration%20Testers/Understanding%20Note-Taking/Taking%20Screenshots.md)
>     - [Tools to Take Screenshots](https://github.com/caderob/hacker-cheatsheet/blob/main/Report%20Writing%20for%20Penetration%20Testers/Understanding%20Note-Taking/Tools%20to%20Take%20Screenshots.md)
>   - [Writing Effective Technical Penetration Testing Reports](https://github.com/caderob/hacker-cheatsheet/tree/main/Report%20Writing%20for%20Penetration%20Testers/Writing%20Effective%20Technical%20Penetration%20Testing%20Reports)
>     - [Executive Summary](https://github.com/caderob/hacker-cheatsheet/blob/main/Report%20Writing%20for%20Penetration%20Testers/Writing%20Effective%20Technical%20Penetration%20Testing%20Reports/Executive%20Summary.md)
>     - [Technical Summary](https://github.com/caderob/hacker-cheatsheet/blob/main/Report%20Writing%20for%20Penetration%20Testers/Writing%20Effective%20Technical%20Penetration%20Testing%20Reports/Technical%20Summary.md)
>     - [Appendices, Further Information, and References](https://github.com/caderob/hacker-cheatsheet/blob/main/Report%20Writing%20for%20Penetration%20Testers/Writing%20Effective%20Technical%20Penetration%20Testing%20Reports/Appendices%2C%20Further%20Information%2C%20and%20References.md)
>- [**Information Gathering**](https://github.com/caderob/hacker-cheatsheet/tree/main/Information%20Gathering)
>   - [Passive Information Gathering](https://github.com/caderob/hacker-cheatsheet/tree/main/Information%20Gathering/Passive%20Information%20Gathering)
>     - [Whois Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Passive%20Information%20Gathering/Whois%20Enumeration.md)
>     - [Google Hacking](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Passive%20Information%20Gathering/Google%20Hacking.md)
>     - [Netcraft (Wappalyzer)](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Passive%20Information%20Gathering/Netcraft%20(Wappalyzer).md)
>     - [Open-Source Code](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Passive%20Information%20Gathering/Open-Source%20Code.md)
>     - [Shodan](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Passive%20Information%20Gathering/Shodan.md)
>     - [Security Headers and SSL/TLS](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Passive%20Information%20Gathering/Security%20Headers%20and%20SSL-TLS.md)
>     - [Passive LLM-Aided Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Passive%20Information%20Gathering/Passive%20LLM-Aided%20Enumeration.md)
>   - [Active Information Gathering](https://github.com/caderob/hacker-cheatsheet/tree/main/Information%20Gathering/Active%20Information%20Gathering)
>     - [DNS Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Active%20Information%20Gathering/DNS%20Enumeration.md)
>     - [TCP/UDP Port Scanning Theory](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Active%20Information%20Gathering/TCP-UDP%20Port%20Scanning%20Theory.md)
>     - [Port Scanning with Nmap](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Active%20Information%20Gathering/Port%20Scanning%20with%20Nmap.md)
>     - [SMB Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Active%20Information%20Gathering/SMB%20Enumeration.md)
>     - [SMTP Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Active%20Information%20Gathering/SMTP%20Enumeration.md)
>     - [SNMP Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Active%20Information%20Gathering/SNMP%20Enumeration.md)
>     - [Active LLM-Aided Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Active%20Information%20Gathering/Active%20LLM-Aided%20Enumeration.md)
>- [**Vulnerability Scanning**]
>   - [Vulnerability Scanning Theory](https://github.com/caderob/hacker-cheatsheet/tree/main/Vulnerability%20Scanning/Vulnerability%20Scanning%20Theory)
>     - [How Vulnerability Scanners Work](https://github.com/caderob/hacker-cheatsheet/blob/main/Vulnerability%20Scanning/Vulnerability%20Scanning%20Theory/How%20Vulnerability%20Scanners%20Work.md)
>     - [Types of Vulnerability Scans](https://github.com/caderob/hacker-cheatsheet/blob/main/Vulnerability%20Scanning/Vulnerability%20Scanning%20Theory/Types%20of%20Vulnerability%20Scans.md)
>     - [Things to consider in a Vulnerability Scan](https://github.com/caderob/hacker-cheatsheet/blob/main/Vulnerability%20Scanning/Vulnerability%20Scanning%20Theory/Things%20to%20consider%20in%20a%20Vulnerability%20Scan.md)
>   - [Vulnerability Scanning with Nessus](https://github.com/caderob/hacker-cheatsheet/tree/main/Vulnerability%20Scanning/Vulnerability%20Scanning%20with%20Nessus)
>     - [Installing Nessus](https://github.com/caderob/hacker-cheatsheet/blob/main/Vulnerability%20Scanning/Vulnerability%20Scanning%20with%20Nessus/Installing%20Nessus.md)
>     - [Nessus Components](https://github.com/caderob/hacker-cheatsheet/blob/main/Vulnerability%20Scanning/Vulnerability%20Scanning%20with%20Nessus/Nessus%20Components.md)
>     - [Performing a Vulnerability Scan](https://github.com/caderob/hacker-cheatsheet/blob/main/Vulnerability%20Scanning/Vulnerability%20Scanning%20with%20Nessus/Performing%20a%20Vulnerability%20Scan.md)
>     - [Analyzing the Results.md](https://github.com/caderob/hacker-cheatsheet/blob/main/Vulnerability%20Scanning/Vulnerability%20Scanning%20with%20Nessus/Analyzing%20the%20Results.md)
>     - [Performing an Authenticated Vulnerability Scan](https://github.com/caderob/hacker-cheatsheet/blob/main/Vulnerability%20Scanning/Vulnerability%20Scanning%20with%20Nessus/Performing%20an%20Authenticated%20Vulnerability%20Scan.md)
>     - [Working with Nessus Plugins](https://github.com/caderob/hacker-cheatsheet/blob/main/Vulnerability%20Scanning/Vulnerability%20Scanning%20with%20Nessus/Working%20with%20Nessus%20Plugins.md)
>   - [Vulnerability Scanning with Nmap]
>     - [NSE Vulnerability Scripts.md](https://github.com/caderob/hacker-cheatsheet/blob/main/Vulnerability%20Scanning/Vulnerability%20Scanning%20with%20Nmap/NSE%20Vulnerability%20Scripts.md)
>     - [Working with NSE Scripts.md]
>- [**Introduction to Web Application Attacks**](https://github.com/caderob/hacker-cheatsheet/tree/main/Introduction%20to%20Web%20Application%20Attacks)
>   - [Web Application Assessment Tools](https://github.com/caderob/hacker-cheatsheet/tree/main/Introduction%20to%20Web%20Application%20Attacks/Web%20Application%20Assessment%20Tools)
>     - [Fingerprinting Web Servers with Nmap](https://github.com/caderob/hacker-cheatsheet/blob/main/Introduction%20to%20Web%20Application%20Attacks/Web%20Application%20Assessment%20Tools/Fingerprinting%20Web%20Servers%20with%20Nmap.md)
>     - [Directory Brute Force with Gobuster](https://github.com/caderob/hacker-cheatsheet/blob/main/Introduction%20to%20Web%20Application%20Attacks/Web%20Application%20Assessment%20Tools/Directory%20Brute%20Force%20with%20Gobuster.md)
>     - [Security Testing with Burp Suite](https://github.com/caderob/hacker-cheatsheet/blob/main/Introduction%20to%20Web%20Application%20Attacks/Web%20Application%20Assessment%20Tools/Security%20Testing%20with%20Burp%20Suite.md)
>   - [Web Application Enumeration](https://github.com/caderob/hacker-cheatsheet/tree/main/Introduction%20to%20Web%20Application%20Attacks/Web%20Application%20Enumeration)
>     - [Debugging Page Content](https://github.com/caderob/hacker-cheatsheet/blob/main/Introduction%20to%20Web%20Application%20Attacks/Web%20Application%20Enumeration/Debugging%20Page%20Content.md)
>     - [Inspecting HTTP Response Headers and Sitemaps](https://github.com/caderob/hacker-cheatsheet/blob/main/Introduction%20to%20Web%20Application%20Attacks/Web%20Application%20Enumeration/Inspecting%20HTTP%20Response%20Headers%20and%20Sitemaps.md)
>     - [Enumerating and Abusing APIs](https://github.com/caderob/hacker-cheatsheet/blob/main/Introduction%20to%20Web%20Application%20Attacks/Web%20Application%20Enumeration/Enumerating%20and%20Abusing%20APIs.md)
>   - [Cross-Site Scripting](https://github.com/caderob/hacker-cheatsheet/tree/main/Introduction%20to%20Web%20Application%20Attacks/Cross-Site%20Scripting)
>     - [Basic XSS](https://github.com/caderob/hacker-cheatsheet/blob/main/Introduction%20to%20Web%20Application%20Attacks/Cross-Site%20Scripting/Basic%20XSS.md)
>     - [Privilege Escalation via XSS](https://github.com/caderob/hacker-cheatsheet/blob/main/Introduction%20to%20Web%20Application%20Attacks/Cross-Site%20Scripting/Privilege%20Escalation%20via%20XSS.md)
>- [**Common Web Application Attacks**]
>   - [Directory Traversal]
>     - [Absolute vs Relative Paths](https://github.com/caderob/hacker-cheatsheet/blob/main/Common%20Web%20Application%20Attacks/Directory%20Traversal/Absolute%20vs%20Relative%20Paths.md)
>     - [Identifying and Exploiting Directory Traversals](https://github.com/caderob/hacker-cheatsheet/blob/main/Common%20Web%20Application%20Attacks/Directory%20Traversal/Identifying%20and%20Exploiting%20Directory%20Traversals.md)
>     - [Encoding Special Characters]
>   - [File Inclusion Vulnerabilities]
>     - [Local File Inclusion (LFI)]
>     - [PHP Wrappers](https://github.com/caderob/hacker-cheatsheet/blob/main/Common%20Web%20Application%20Attacks/File%20Inclusion%20Vulnerabilities/PHP%20Wrappers.md)
>     - [Remote File Inclusion (RFI)]
>   - [File Upload Vulnerabilities]
>     - [Using Executable Files]
>     - [Using Non-Executable Files]
>   - [Command Injection](https://github.com/caderob/hacker-cheatsheet/tree/main/Common%20Web%20Application%20Attacks/Command%20Injection)
>     - [OS Command Injection](https://github.com/caderob/hacker-cheatsheet/blob/main/Common%20Web%20Application%20Attacks/Command%20Injection/OS%20Command%20Injection.md)
>- [**SQL Injection Attacks**]
>   - [SQL Theory and Databases]
>     - [SQL Theory Refresher](https://github.com/caderob/hacker-cheatsheet/blob/main/SQL%20Injection%20Attacks/SQL%20Theory%20and%20Databases/SQL%20Theory%20Refresher.md)
>     - [DB Types and Characteristics]
>   - [Manual SQL Exploitation]
>     - [Identifying SQLi via Error-based Payloads](https://github.com/caderob/hacker-cheatsheet/blob/main/SQL%20Injection%20Attacks/Manual%20SQL%20Exploitation/Identifying%20SQLi%20via%20Error-based%20Payloads.md)
>     - [UNION-based Payloads](https://github.com/caderob/hacker-cheatsheet/blob/main/SQL%20Injection%20Attacks/Manual%20SQL%20Exploitation/UNION-based%20Payloads.md)
>     - [Blind SQL Injections]
>   - [Manual and Automated Code Execution]
>     - [Manual Code Execution](https://github.com/caderob/hacker-cheatsheet/blob/main/SQL%20Injection%20Attacks/Manual%20and%20Automated%20Code%20Execution/Manual%20Code%20Execution.md)
>     - [Automating the Attack]
>- [**Phishing Basics**]
>   - [Phishing 101](https://github.com/caderob/hacker-cheatsheet/tree/main/Phishing%20Basics/Phishing%20101)
>     - [LLMs, Generative AI, and Deepfakes](https://github.com/caderob/hacker-cheatsheet/blob/main/Phishing%20Basics/Phishing%20101/LLMs%2C%20Generative%20AI%2C%20and%20Deepfakes.md)
>   - [Payloads, Misdirection, and Speedbumps](https://github.com/caderob/hacker-cheatsheet/tree/main/Phishing%20Basics/Payloads%2C%20Misdirection%2C%20and%20Speedbumps)
>     - [Identifying Risks of Malicious Office Macros](https://github.com/caderob/hacker-cheatsheet/blob/main/Phishing%20Basics/Payloads%2C%20Misdirection%2C%20and%20Speedbumps/Identifying%20Risks%20of%20Malicious%20Office%20Macros.md)
>     - [Differentiate Credential Phishing and Multi-Factor Authentication (MFA)](https://github.com/caderob/hacker-cheatsheet/blob/main/Phishing%20Basics/Payloads,%20Misdirection,%20and%20Speedbumps/Differentiate%20Credential%20Phishing%20and%20Multi-Factor%20Authentication%20(MFA).md)
>   - [Hands-On Credential Phishing]
>     - [Creating a Zoom Credential Phishing Pretext]
>     - [Cloning a Legitimate Website]
>     - [Cleaning Up the Clone]
>     - [Injecting Malicious Elements in the Clone]
>     - [Crafting the Phishing email]
>- [**Client-side Attacks**]
>   - [Target Reconnaissance](https://github.com/caderob/hacker-cheatsheet/tree/main/Client-side%20Attacks/Target%20Reconnaissance)
>     - [Information Gathering](https://github.com/caderob/hacker-cheatsheet/blob/main/Client-side%20Attacks/Target%20Reconnaissance/Information%20Gathering.md)
>     - [Client Fingerprinting](https://github.com/caderob/hacker-cheatsheet/blob/main/Client-side%20Attacks/Target%20Reconnaissance/Client%20Fingerprinting.md)
>   - [Exploiting Microsoft Office]
>     - [Preparing the Attack](https://github.com/caderob/hacker-cheatsheet/blob/main/Client-side%20Attacks/Exploiting%20Microsoft%20Office/Preparing%20the%20Attack.md)
>     - [Installing Microsoft Office](https://github.com/caderob/hacker-cheatsheet/blob/main/Client-side%20Attacks/Exploiting%20Microsoft%20Office/Installing%20Microsoft%20Office.md)
>     - [Leveraging Microsoft Word Macros]
>   - [Abusing Windows Library Files]
>     - [Obtaining Code Execution via Windows Library Files]
>- [**Locating Public Exploits**]
>   - [Getting Started](https://github.com/caderob/hacker-cheatsheet/tree/main/Locating%20Public%20Exploits/Getting%20Started)
>     - [A Word of Caution](https://github.com/caderob/hacker-cheatsheet/blob/main/Locating%20Public%20Exploits/Getting%20Started/A%20Word%20of%20Caution.md)
>   - [Online Exploit Resources](https://github.com/caderob/hacker-cheatsheet/tree/main/Locating%20Public%20Exploits/Online%20Exploit%20Resources)
>     - [The Exploit Database](https://github.com/caderob/hacker-cheatsheet/blob/main/Locating%20Public%20Exploits/Online%20Exploit%20Resources/The%20Exploit%20Database.md)
>     - [Packet Storm](https://github.com/caderob/hacker-cheatsheet/blob/main/Locating%20Public%20Exploits/Online%20Exploit%20Resources/Packet%20Storm.md)
>     - [GitHub](https://github.com/caderob/hacker-cheatsheet/blob/main/Locating%20Public%20Exploits/Online%20Exploit%20Resources/GitHub.md)
>     - [Google Search Operators](https://github.com/caderob/hacker-cheatsheet/blob/main/Locating%20Public%20Exploits/Online%20Exploit%20Resources/Google%20Search%20Operators.md)
>   - [Offline Exploit Resources](https://github.com/caderob/hacker-cheatsheet/tree/main/Locating%20Public%20Exploits/Offline%20Exploit%20Resources)
>     - [Exploit Frameworks](https://github.com/caderob/hacker-cheatsheet/blob/main/Locating%20Public%20Exploits/Offline%20Exploit%20Resources/Exploit%20Frameworks.md)
>     - [SearchSploit](https://github.com/caderob/hacker-cheatsheet/blob/main/Locating%20Public%20Exploits/Offline%20Exploit%20Resources/SearchSploit.md)
>     - [Nmap NSE Scripts](https://github.com/caderob/hacker-cheatsheet/blob/main/Locating%20Public%20Exploits/Offline%20Exploit%20Resources/Nmap%20NSE%20Scripts.md)
>   - [Exploiting a Target]
>     - [Putting It Together]
>- [**Fixing Exploits**]
>   - [Fixing Memory Corruption Exploits]
>     - [Buffer Overflow in a Nutshell]
>     - [Importing and Examining the Exploit]
>     - [Cross-Compiling Exploit Code]
>     - [Fixing the Exploit]
>     - [Changing the Overflow Buffer]
>   - [Fixing Web Exploits]
>     - [Selecting the Vulnerability and Fixing the Code]
>     - [Troubleshooting the "index out of range" Error]
>- [**Antivirus Evasion**]
>   - [Antivirus Software Key Components and Operations]
>     - [Detection Methods]
>   - [Bypassing Antivirus Detections]
>     - [In-Memory Evasion]
>   - [AV Evasion in Practice]
>     - [Testing for AV Evasion]
>     - [Evading AV with Thread Injection]
>     - [Automating the Process]
>- [**Password Attacks**]
>   - [Attacking Network Services Logins]
>     - [SSH]
>     - [RDP]
>     - [HTTP POST Login Form]
>   - [Password Cracking Fundamentals]
>     - [Introduction to Encryption, Hashes and Cracking]
>     - [Mutating Wordlists]
>     - [Cracking Methodology]
>     - [Password Manager]
>     - [SSH Private Key Passphrase]
>   - [Working with Password Hashes]
>     - [Cracking NTLM]
>     - [Passing NTLM]
>     - [Cracking Net-NTLMv2]
>     - [Relaying Net-NTLMv2]
>     - [Windows Credential Guard]
>- [**Windows Privilege Escalation**]
>   - [Enumerating Windows]
>     - [Understanding Windows Privileges and Access Control Mechanisms]
>     - [Situational Awareness]
>     - [Hidden in Plain View]
>     - [Information Goldmine PowerShell]
>     - [Automated Enumeration]
>   - [Leveraging Windows Services]
>     - [Service Binary Hijacking]
>     - [DLL Hijacking]
>     - [Unquoted Service Paths]
>   - [Abusing Other Windows Components]
>     - [Scheduled Tasks]
>     - [Using Exploits]
>- [**Linux Privilege Escalation**]
>   - [Enumerating Linux]
>     - [Understanding Files and Users Privileges on Linux]
>     - [Manual Enumeration]
>     - [Automated Enumeration]
>   - [Exposed Confidential Information]
>     - [Inspecting User Trails]
>     - [Inspecting Service Footprints]
>   - [Insecure File Permissions]
>     - [Abusing Cron Jobs]
>     - [Abusing Password Authentication]
>   - [Insecure System Components]
>     - [Abusing Setuid Binaries and Capabilities]
>     - [Abusing Sudo]
>     - [Exploiting Kernel Vulnerabilities]
>- [**Port Redirection and SSH Tunneling**]
>- [**Tunneling Through Deep Packet Inspection**]
>- [**The Metasploit Framework**]
>- [**Active Directory Introduction and Enumeration**]
>- [**Attacking Active Directory Authentication**]
>- [**Lateral Movement in Active Directory**]
>- [**Enumerating AWS Cloud Infrastructure**]
>- [**Attacking AWS Cloud Infrastructure**]
>- [**Assembling the Pieces**]
