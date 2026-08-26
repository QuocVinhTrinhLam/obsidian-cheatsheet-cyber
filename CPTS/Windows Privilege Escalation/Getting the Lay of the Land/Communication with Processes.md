## Enumerating Network Services
#### Display Active Network Connections

```cmd
C:\htb> netstat -ano
```
## Named Pipes
#### Listing Named Pipes with Pipelist

```cmd
C:\htb> pipelist.exe /accepteula
```
![](Screenshot%202026-08-18%20at%2016.19.04.png)
#### Listing Named Pipes with PowerShell

```powershell
PS C:\htb> gci \\.\pipe\
```
#### Reviewing LSASS Named Pipe Permissions

```cmd
C:\htb> accesschk.exe /accepteula \\.\Pipe\lsass -v
```
## Named Pipes Attack Example
#### Checking WindscribeService Named Pipe Permissions

```cmd
C:\htb> accesschk.exe -accepteula -w \pipe\WindscribeService -v

Accesschk v6.13 - Reports effective permissions for securable objects
Copyright ⌐ 2006-2020 Mark Russinovich
Sysinternals - www.sysinternals.com

\\.\Pipe\WindscribeService
  Medium Mandatory Level (Default) [No-Write-Up]
  RW Everyone
        FILE_ALL_ACCESS
```
