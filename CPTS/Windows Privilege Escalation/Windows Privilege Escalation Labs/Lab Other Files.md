# [Other Files](Other%20Files.md)
### Using the techniques shown in this section, find the cleartext password for the bob_adm user on the target system.

```powershell
Get-ChildItem "C:\Users\htb-student\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState" -ErrorAction SilentlyContinue
```
![](Screenshot%202026-08-24%20at%2009.28.19.png)

```powershell
cd C:\Users\htb-student\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState
findstr /I "bob_adm password admin" plum.sqlite
```
![](Screenshot%202026-08-24%20at%2009.29.29.png)