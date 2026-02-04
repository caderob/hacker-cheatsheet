# Analysing Data using BloodHound

Starting the Neo4j service in Kali Linux
>``` shell
>kali@kali:~$ sudo neo4j start
>
># ========== Expected Result ==========
>Directories in use:
>home:         /usr/share/neo4j
>config:       /usr/share/neo4j/conf
>logs:         /usr/share/neo4j/logs
>plugins:      /usr/share/neo4j/plugins
>import:       /usr/share/neo4j/import
>data:         /usr/share/neo4j/data
>certificates: /usr/share/neo4j/certificates
>licenses:     /usr/share/neo4j/licenses
>run:          /usr/share/neo4j/run
>Starting Neo4j.
>Started neo4j (pid:334819). It is available at http://localhost:7474
>There may be a short delay until the server is ready.
># =====================================
>```

Neo4j First Login
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Analysing-Data-using-BloodHound-1.png)

Neo4j Password Change
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Analysing-Data-using-BloodHound-2.png)

Starting BloodHound in Kali Linux
>``` shell
>kali@kali:~$ bloodhound
>```

BloodHound Login
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Analysing-Data-using-BloodHound-3.png)

Uploading Collected Data
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Analysing-Data-using-BloodHound-4.png)

BloodHound DB Info
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Analysing-Data-using-BloodHound-5.png)

BloodHound Analysis Overview
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Analysing-Data-using-BloodHound-6.png)

BloodHound Domain Admins
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Analysing-Data-using-BloodHound-7.png)

BloodHound Node Display
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Analysing-Data-using-BloodHound-8.png)

BloodHound Node Display2
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Analysing-Data-using-BloodHound-9.png)

BloodHound Shortest Path DA
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Analysing-Data-using-BloodHound-10.png)

BloodHound Stephanie RDP
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Analysing-Data-using-BloodHound-11.png)

BloodHound Help
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Analysing-Data-using-BloodHound-12.png)

BloodHound Mark Owned
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Analysing-Data-using-BloodHound-13.png)

BloodHound Shortest Path DA from Owned Principals
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Analysing-Data-using-BloodHound-14.png)

Lab 1 - If you have not collected data using SharpHound at this point, start VM Group 1 and perform the data collection. Transfer the .zip file generated with SharpHound to Kali Linux. Start BloodHound and repeat the analysis steps outlined in this section to find the promising attack path. Which service does BloodHound rely on to display the data in graphs?
>``` shell
>
>```
>

Lab 2 - Search for the Management Department group in BloodHound and use the Node Info tab to have a look at the Inbound Control Rights for the group. What group is currently the owner of the Management Department group? Submit the answer without the domain name (@corp.com).
>``` shell
>
>```
>

Lab 3 - Capstone Exercise: Start VM Group 2 and log in as stephanie to CLIENT75. From CLIENT75, enumerate the object permissions for the domain users. Once weak permissions have been identified, use them to take full control over the account and use it to log in to the domain. Once logged in, repeat the enumeration process using techniques shown in this Module to obtain the flag.
>``` shell
>
>```
>
