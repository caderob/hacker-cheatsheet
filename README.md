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
>     - [Open-Source Code (GitHub, GitHub Gist, GitLab, SourceForge)](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Passive%20Information%20Gathering/Open-Source%20Code%20(GitHub%2C%20GitHub%20Gist%2C%20GitLab%2C%20SourceForge).md)
>     - [Passive LLM-Aided Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Passive%20Information%20Gathering/Passive%20LLM-Aided%20Enumeration.md)
>   - [Active Information Gathering](https://github.com/caderob/hacker-cheatsheet/tree/main/Information%20Gathering/Active%20Information%20Gathering)
>     - [DNS Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Active%20Information%20Gathering/DNS%20Enumeration.md)
>     - [TCP-UDP Port Scanning Theory](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Active%20Information%20Gathering/TCP%20UDP%20Port%20Scanning%20Theory.md)
>     - [Port Scanning with Nmap](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Active%20Information%20Gathering/Port%20Scanning%20with%20Nmap.md)
>     - [SMB Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Active%20Information%20Gathering/SMB%20Enumeration.md)
>     - [SMTP Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Active%20Information%20Gathering/SMTP%20Enumeration.md)
>     - [SNMP Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Active%20Information%20Gathering/SNMP%20Enumeration.md)
>     - [Active LLM-Aided Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/Information%20Gathering/Active%20Information%20Gathering/Active%20LLM-Aided%20Enumeration.md)
>- [**Vulnerability Scanning**](https://github.com/caderob/hacker-cheatsheet/tree/main/Vulnerability%20Scanning)
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
>   - [Vulnerability Scanning with Nmap](https://github.com/caderob/hacker-cheatsheet/tree/main/Vulnerability%20Scanning/Vulnerability%20Scanning%20with%20Nmap)
>     - [NSE Vulnerability Scripts.md](https://github.com/caderob/hacker-cheatsheet/blob/main/Vulnerability%20Scanning/Vulnerability%20Scanning%20with%20Nmap/NSE%20Vulnerability%20Scripts.md)
>     - [Working with NSE Scripts.md]
>- [**Introduction to Web Application Attacks**]
>   - [Web Application Assessment Tools]
>     - [Fingerprinting Web Servers with Nmap]
>     - [Directory Brute Force with Gobuster]
>     - [Security Testing with Burp Suite]
>   - [Web Application Enumeration]
>   - [Cross-Site Scripting]
