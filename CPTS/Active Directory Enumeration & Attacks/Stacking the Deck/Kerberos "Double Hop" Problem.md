![](Screenshot%202026-05-30%20at%2015.20.28.png)
![](Pasted%20image%2020260530152127.png)
## Workaround #1: PSCredential Object

```powershell
PS C:\Users\backupadm\Documents> import-module .\PowerView.ps1
```

```powershell
PS C:\Users\backupadm\Documents> klist
```
![](Screenshot%202026-05-30%20at%2015.25.10.png)

```powershell
PS C:\Users\backupadm\Documents> $SecPassword = ConvertTo-SecureString '!qazXSW@' -AsPlainText -Force
```

```powershell
PS C:\Users\backupadm\Documents> get-domainuser -spn -credential $Cred | select samaccountname
```

```powershell
get-domainuser -spn | select 
*Evil-WinRM* PS C:\Users\backupadm\Documents> get-domainuser -spn | select samaccountname
```
## Workaround #2: Register PSSession Configuration

```powershell
PS C:\htb> Enter-PSSession -ComputerName ACADEMY-AEN-DEV01.INLANEFREIGHT.LOCAL -Credential inlanefreight\backupadm
```

```powershell
PS C:\Users\backupadm\Documents> klist
```
![](Screenshot%202026-05-30%20at%2015.39.49.png)

```powershell
[ACADEMY-AEN-DEV01.INLANEFREIGHT.LOCAL]: PS C:\Users\backupadm\Documents> Import-Module .\PowerView.ps1 
[ACADEMY-AEN-DEV01.INLANEFREIGHT.LOCAL]: PS C:\Users\backupadm\Documents> get-domainuser -spn | select samaccountname
```

```powershell
PS C:\htb> Register-PSSessionConfiguration -Name backupadmsess -RunAsCredential inlanefreight\backupadm
```

Once this is done, we need to restart the WinRM service by typing `Restart-Service WinRM` in our current PSSession. This will kick us out, so we'll start a new PSSession using the named registered session we set up previously.

```powershell
PS C:\htb> Enter-PSSession -ComputerName DEV01 -Credential INLANEFREIGHT\backupadm -ConfigurationName backupadmsess 
[DEV01]: PS C:\Users\backupadm\Documents> klist
```

```powershell
[DEV01]: PS C:\Users\Public> get-domainuser -spn | select samaccountname
```
