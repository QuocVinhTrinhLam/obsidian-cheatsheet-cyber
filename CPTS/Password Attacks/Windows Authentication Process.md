#### Windows authentication process diagram
![[Pasted image 20260507122419.png]]
#### LSASS
|**Authentication Packages**|**Description**|
|---|---|
|`Lsasrv.dll`|The LSA Server service both enforces security policies and acts as the security package manager for the LSA. The LSA contains the Negotiate function, which selects either the NTLM or Kerberos protocol after determining which protocol is to be successful.|
|`Msv1_0.dll`|Authentication package for local machine logons that don't require custom authentication.|
|`Samsrv.dll`|The Security Accounts Manager (SAM) stores local security accounts, enforces locally stored policies, and supports APIs.|
|`Kerberos.dll`|Security package loaded by the LSA for Kerberos-based authentication on a machine.|
|`Netlogon.dll`|Network-based logon service.|
|`Ntdsa.dll`|Directory System Agent (DSA) that manages the Active Directory database (ntds.dit), processes LDAP queries, and handles replication between domain controllers. Only loaded on Domain Controllers.|
#### SAM database
![[Pasted image 20260507123213.png]]
#### Credential Manager
![[Pasted image 20260507123219.png]]

```powershell
PS C:\Users\[Username]\AppData\Local\Microsoft\[Vault/Credentials]\
```
#### NTDS

`NTDS.dit` is a database file that stores Active Directory data, including but not limited to:

- User accounts (username & password hash)
- Group accounts
- Computer accounts
- Group policy objects
