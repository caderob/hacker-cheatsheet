# Hacker Cheatsheet

This repository serves as a comprehensive compilation of notes and commands assembled during my preparation for the Offensive Security Certified Professional (OSCP) certification, specifically the PEN-200 course undertaken in 2025. 

The purpose of this cheatsheet is to provide a structured and concise reference to aid in the understanding and execution of various penetration testing techniques covered in the OSCP curriculum.

## Contents

>- [**Information Gathering**](https://github.com/caderob/hacker-cheatsheet/tree/main/01%20Information%20Gathering)
>   - [Passive Information Gathering](https://github.com/caderob/hacker-cheatsheet/tree/main/01%20Information%20Gathering/01%20Passive%20Information%20Gathering)
>     - [Whois Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/01%20Information%20Gathering/01%20Passive%20Information%20Gathering/01%20Whois%20Enumeration.md)
>     - [Google Hacking](https://github.com/caderob/hacker-cheatsheet/blob/main/01%20Information%20Gathering/01%20Passive%20Information%20Gathering/02%20Google%20Hacking.md)
>     - [Netcraft (Wappalyzer)](https://github.com/caderob/hacker-cheatsheet/blob/main/01%20Information%20Gathering/01%20Passive%20Information%20Gathering/03%20Netcraft%20(Wappalyzer).md)
>     - [Open-Source Code (GitHub, GitHub Gist, GitLab, SourceForge)](https://github.com/caderob/hacker-cheatsheet/blob/main/01%20Information%20Gathering/01%20Passive%20Information%20Gathering/04%20Open-Source%20Code%20(GitHub%2C%20GitHub%20Gist%2C%20GitLab%2C%20SourceForge).md)
>     - [Passive LLM-Aided Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/01%20Information%20Gathering/01%20Passive%20Information%20Gathering/05%20Passive%20LLM-Aided%20Enumeration.md)
>   - [Active Information Gathering](https://github.com/caderob/hacker-cheatsheet/tree/main/01%20Information%20Gathering/02%20Active%20Information%20Gathering)
>     - [DNS Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/01%20Information%20Gathering/02%20Active%20Information%20Gathering/01%20DNS%20Enumeration.md)
>     - [TCP UDP Port Scanning Theory](https://github.com/caderob/hacker-cheatsheet/blob/main/01%20Information%20Gathering/02%20Active%20Information%20Gathering/02%20TCP%20UDP%20Port%20Scanning%20Theory.md)
>     - [Port Scanning with Nmap](https://github.com/caderob/hacker-cheatsheet/blob/main/01%20Information%20Gathering/02%20Active%20Information%20Gathering/03%20Port%20Scanning%20with%20Nmap.md)
>     - [SMB Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/01%20Information%20Gathering/02%20Active%20Information%20Gathering/04%20SMB%20Enumeration.md)
>     - [SMTP Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/01%20Information%20Gathering/02%20Active%20Information%20Gathering/05%20SMTP%20Enumeration.md)
>     - [SNMP Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/01%20Information%20Gathering/02%20Active%20Information%20Gathering/06%20SNMP%20Enumeration.md)
>     - [Active LLM-Aided Enumeration](https://github.com/caderob/hacker-cheatsheet/blob/main/01%20Information%20Gathering/02%20Active%20Information%20Gathering/07%20Active%20LLM-Aided%20Enumeration.md)
>- **Vulnerability Scanning**
>   - Vulnerability Scanning with Nessus
>   - Vulnerability Scanning with Nmap
>- **Introduction to Web Application Attacks**
>   - Web Application Assessment Tools
>   - Web Application Enumeration
>   - Cross-Site Scripting
>- **Common Web Application Attacks**
>   - Directory Traversal
>   - File Inclusion Vulnerabilities
>   - File Upload Vulnerabilities
>   - Command Injection
>- **SQL Injection Attacks**
>   - SQL Theory and Databases
>   - Manual SQL Exploitation
>   - Manual and Automated Code Execution
>- **Client-side Attacks**
>   - Target Reconnaissance
>   - Exploiting Microsoft Office
>   - Abusing Windows Library Files
>- **Locating Public Exploits**
>   - Getting Started
>   - Online Exploit Resources
>   - Offline Exploit Resources
>   - Exploiting a Target
>- **Fixing Exploits**
>   - Fixing Memory Corruption Exploits
>   - Fixing Web Exploits
>- **Phishing Basics**
>   - Phishing 101
>   - Payloads, Misdirection, and Speedbumps
>   - Hands-On Credential Phishing
>- **Antivirus Evasion**
>   - Antivirus Software Key Components and Operations
>   - Bypassing Antivirus Detections
>   - AV Evasion in Practice
>- **Password Attacks**
>   - Attacking Network Services Logins
>   - Password Cracking Fundamentals
>   - Working with Password Hashes
>- **Windows Privilege Escalation**
>   - Enumerating Windows
>   - Leveraging Windows Services
>   - Abusing Other Windows Components
>- **Linux Privilege Escalation**
>   - Enumerating Linux
>   - Exposed Confidential Information
>   - Insecure File Permissions
>   - Insecure System Components
>- **Port Redirection and SSH Tunneling**
>   - Why Port Redirection and Tunneling?
>   - Port Forwarding with Linux Tools
>   - SSH Tunneling
>   - Port Forwarding with Windows Tools
>- **Tunneling Through Deep Packet Inspection**
>   - HTTP Tunneling Theory and Practice
>   - DNS Tunneling Theory and Practice
>- **The Metasploit Framework**
>   - Getting Familiar with Metasploit
>   - Using Metasploit Payloads
>   - Performing Post-Exploitation with Metasploit
>   - Automating Metasploit
>- **Active Directory Introduction and Enumeration**
>   - Active Directory - Introduction
>   - Active Directory - Manual Enumeration
>   - Manual Enumeration - Expanding our Repertoire
>   - Active Directory - Automated Enumeration
>- **Attacking Active Directory Authentication**
>   - Understanding Active Directory Authentication
>   - Performing Attacks on Active Directory Authentication
>- **Lateral Movement in Active Directory**
>   - Active Directory Lateral Movement Techniques
>   - Active Directory Persistence
>- **Enumerating AWS Cloud Infrastructure**
>   - About the Public Cloud Labs
>   - Reconnaissance of Cloud Resources on the Internet
>   - Reconnaissance via Cloud Service Provider's API
>   - Initial IAM Reconnaissance
>   - IAM Resources Enumeration
>- **Attacking AWS Cloud Infrastructure**
>   - About the Public Cloud Labs
>   - Leaked Secrets to Poisoned Pipeline - Lab Design
>   - Enumeration
>   - Discovering Secrets
>   - Poisoning the Pipeline
>   - Compromising the Environment via Backdoor Account
>   - Dependency Chain Abuse
>   - Information Gathering
>   - Dependency Chain Attack
>   - Compromising the Environment
