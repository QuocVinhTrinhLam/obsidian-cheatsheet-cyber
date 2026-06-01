## Enumerating the Password Policy - from Linux - Credentialed

```shell
3kjS@htb[/htb]$ crackmapexec smb 172.16.5.5 -u avazquez -p Password123 --pass-pol
```
## Enumerating the Password Policy - from Linux - SMB NULL Sessions
#### Using rpcclient

```shell
3kjS@htb[/htb]$ rpcclient -U "" -N 172.16.5.5
```
![[Screenshot 2026-05-25 at 13.02.47.png]]
#### Obtaining the Password Policy using rpcclient

```shell
rpcclient $> querydominfo
```
![[Screenshot 2026-05-25 at 13.03.07.png]]

| Tool      | Ports                                             |
| --------- | ------------------------------------------------- |
| nmblookup | 137/UDP                                           |
| nbtstat   | 137/UDP                                           |
| net       | 139/TCP, 135/TCP, TCP and UDP 135 and 49152-65535 |
| rpcclient | 135/TCP                                           |
| smbclient | 445/TCP                                           |
#### Using enum4linux

```shell
3kjS@htb[/htb]$ enum4linux -P 172.16.5.5
```
#### Using enum4linux-ng

```shell
3kjS@htb[/htb]$ enum4linux-ng -P 172.16.5.5 -oA ilfreight
```
#### Displaying the contents of ilfreight.json

```shell
3kjS@htb[/htb]$ cat ilfreight.json
```
## Enumerating Null Session - from Windows

`net use \\host\ipc$ "" /u:""`
#### Establish a null session from windows

```cmd
C:\htb> net use \\DC01\ipc$ "" /u:"" 
The command completed successfully.
```
#### Error: Account is Disabled

```cmd
C:\htb> net use \\DC01\ipc$ "" /u:guest 
System error 1331 has occurred. 

This user can't sign in because this account is currently disabled.
```
#### Error: Password is Incorrect

```cmd
C:\htb> net use \\DC01\ipc$ "password" /u:guest 
System error 1326 has occurred. 

The user name or password is incorrect.
```
#### Error: Account is locked out (Password Policy)

```cmd
C:\htb> net use \\DC01\ipc$ "password" /u:guest 
System error 1909 has occurred. 

The referenced account is currently locked out and may not be logged on to.
```
## Enumerating the Password Policy - from Linux - LDAP Anonymous Bind
#### Using ldapsearch

```shell
3kjS@htb[/htb]$ ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```
Note: In newer versions of `ldapsearch`, the `-h` parameter was deprecated in favor for `-H`.
## Enumerating the Password Policy - from Windows
#### Using net.exe

```cmd
C:\htb> net accounts
```
![[Screenshot 2026-05-25 at 13.15.39.png]]
#### Using PowerView

```cmd
PS C:\htb> import-module .\PowerView.ps1 
PS C:\htb> Get-DomainPolicy
```
![[Screenshot 2026-05-25 at 13.16.14.png]]
