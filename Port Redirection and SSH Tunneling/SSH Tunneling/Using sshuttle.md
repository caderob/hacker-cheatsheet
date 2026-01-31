# Using sshuttle

Forwarding port 2222 on CONFLUENCE01 to port 22 on PGDATABASE01
>``` shell
>confluence@confluence01:/opt/atlassian/confluence/bin$ socat TCP-LISTEN:2222,fork TCP:10.4.50.215:22
>
># ========== Expected Result ==========
></bin$ socat TCP-LISTEN:2222,fork TCP:10.4.50.215:22 
># =====================================
>```

Running sshuttle from our Kali machine, pointing to the forward port on CONFLUENCE01
>``` shell
>kali@kali:~$ sshuttle -r database_admin@192.168.50.63:2222 10.4.50.0/24 172.16.50.0/24
>
># ========== Expected Result ==========
>[local sudo] Password: 
>
>database_admin@192.168.50.63's password: 
>
>c : Connected to server.
>Failed to flush caches: Unit dbus-org.freedesktop.resolve1.service not found.
>fw: Received non-zero return code 1 when flushing DNS resolver cache.
># =====================================
>```

Connecting to the SMB share on HRSHARES, without any explicit forwarding
>``` shell
>kali@kali:~$ smbclient -L //172.16.50.217/ -U hr_admin --password=Welcome1234
>
># ========== Expected Result ==========
>        Sharename       Type      Comment
>        ---------       ----      -------
>        ADMIN$          Disk      Remote Admin
>        C$              Disk      Default share
>        IPC$            IPC       Remote IPC
>        scripts         Disk
>Reconnecting with SMB1 for workgroup listing.
>do_connect: Connection to 172.16.50.217 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
>Unable to connect with SMB1 -- no workgroup available
># =====================================
>```

Lab 1 - True or false: in order to run sshuttle, you need root privileges on the SSH client machine.
>True
