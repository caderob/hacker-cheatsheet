# Enumerating Object Permissions

AD permission types
>``` shell
>GenericAll: Full permissions on object
>GenericWrite: Edit certain attributes on the object
>WriteOwner: Change ownership of the object
>WriteDACL: Edit ACE's applied to object
>AllExtendedRights: Change password, reset password, etc.
>ForceChangePassword: Password change for object
>Self (Self-Membership): Add ourselves to for example a group
>```

Running Get-ObjectAcl specifying our user
>``` shell
>PS C:\Tools> Get-ObjectAcl -Identity stephanie
>
># ========== Expected Result ==========
>...
>ObjectDN               : CN=stephanie,CN=Users,DC=corp,DC=com
>ObjectSID              : S-1-5-21-1987370270-658905905-1781884369-1104
>ActiveDirectoryRights  : ReadProperty
>ObjectAceFlags         : ObjectAceTypePresent
>ObjectAceType          : 4c164200-20c0-11d0-a768-00aa006e0529
>InheritedObjectAceType : 00000000-0000-0000-0000-000000000000
>BinaryLength           : 56
>AceQualifier           : AccessAllowed
>IsCallback             : False
>OpaqueLength           : 0
>AccessMask             : 16
>SecurityIdentifier     : S-1-5-21-1987370270-658905905-1781884369-553
>AceType                : AccessAllowedObject
>AceFlags               : None
>IsInherited            : False
>InheritanceFlags       : None
>PropagationFlags       : None
>AuditFlags             : None
>...
># =====================================
>```

Converting the ObjectISD into name
>``` shell
>PS C:\Tools> Convert-SidToName S-1-5-21-1987370270-658905905-1781884369-1104
>
># ========== Expected Result ==========
>CORP\stephanie
># =====================================
>```

Converting the SecurityIdentifier into name
>``` shell
>PS C:\Tools> Convert-SidToName S-1-5-21-1987370270-658905905-1781884369-553
>
># ========== Expected Result ==========
>CORP\RAS and IAS Servers
># =====================================
>```

Enumerating ACLs for the Management Group
>``` shell
>PS C:\Tools> Get-ObjectAcl -Identity "Management Department" | ? {$_.ActiveDirectoryRights -eq "GenericAll"} | select SecurityIdentifier,ActiveDirectoryRights
>
># ========== Expected Result ==========
>SecurityIdentifier                            ActiveDirectoryRights
>------------------                            ---------------------
>S-1-5-21-1987370270-658905905-1781884369-512             GenericAll
>S-1-5-21-1987370270-658905905-1781884369-1104            GenericAll
>S-1-5-32-548                                             GenericAll
>S-1-5-18                                                 GenericAll
>S-1-5-21-1987370270-658905905-1781884369-519             GenericAll
># =====================================

Converting all SIDs that have GenericAll permission on the Management Group
>``` shell
>PS C:\Tools> "S-1-5-21-1987370270-658905905-1781884369-512","S-1-5-21-1987370270-658905905-1781884369-1104","S-1-5-32-548","S-1-5-18","S-1-5-21-1987370270-658905905-1781884369-519" | Convert-SidToName
>
># ========== Expected Result ==========
>CORP\Domain Admins
>CORP\stephanie
>BUILTIN\Account Operators
>Local System
>CORP\Enterprise Admins
># =====================================
>```

Using "net.exe" to add ourselves to domain group
>``` shell
>PS C:\Tools> net group "Management Department" stephanie /add /domain
>
># ========== Expected Result ==========
>The request will be processed at a domain controller for domain corp.com.
>
>The command completed successfully.
># =====================================
>```

Running "Get-NetGroup" to enumerate "Management Department"
>``` shell
>PS C:\Tools> Get-NetGroup "Management Department" | select member
>
># ========== Expected Result ==========
>member
>------
>{CN=jen,CN=Users,DC=corp,DC=com, CN=stephanie,CN=Users,DC=corp,DC=com}
># =====================================
>```

Using "net.exe" to remove ourselves from domain group
>``` shell
>PS C:\Tools> net group "Management Department" stephanie /del /domain
>
># ========== Expected Result ==========
>The request will be processed at a domain controller for domain corp.com.
>
>The command completed successfully.
># =====================================
>```

Running "Get-NetGroup" to verify that our user is removed from domain group
>``` shell
>PS C:\Tools> Get-NetGroup "Management Department" | select member
>
># ========== Expected Result ==========
>member
>------
>CN=jen,CN=Users,DC=corp,DC=com
># =====================================
>```

Lab 1 - Start VM Group 1 and log in to CLIENT75 as stephanie. Repeat the enumeration steps outlined in this section to get an understanding for the object permissions. What kind of entries makes up an ACL?
>``` shell
>
>```
>

Lab 2 - What is the most powerful ACL we can have on an object in Active Directory?
>``` shell
>
>```
>
