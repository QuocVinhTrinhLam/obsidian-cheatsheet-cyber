## Command Line

```shell
3kjS@htb[/htb]$ mysql -u root -p 

Enter password: <password> 
...SNIP... 

mysql>
```

```shell
3kjS@htb[/htb]$ mysql -u root -h docker.hackthebox.eu -P 3306 -p 

Enter password: 
...SNIP... 

mysql>
```
## Creating a database

```sql
mysql> CREATE DATABASE users; 

Query OK, 1 row affected (0.02 sec)
```

```sql
mysql> SHOW DATABASES;

mysql> USE users; 

Database changed
```
## Tables

```sql
CREATE TABLE logins ( 
	id INT, 
	username VARCHAR(100), 
	password VARCHAR(100), 
	date_of_joining DATETIME 
	);
```
![](Screenshot%202026-06-15%20at%2019.06.30.png)
![](Screenshot%202026-06-15%20at%2019.06.37.png)![](Screenshot%202026-06-15%20at%2019.07.01.png)
#### Table Properties

```sql
	id INT NOT NULL AUTO_INCREMENT,
```

```sql
	username VARCHAR(100) UNIQUE NOT NULL,
```

```sql
	date_of_joining DATETIME DEFAULT NOW(),
```

```sql
	PRIMARY KEY (id)
```

```sql
CREATE TABLE logins ( 
	id INT NOT NULL AUTO_INCREMENT, 
	username VARCHAR(100) UNIQUE NOT NULL, 
	password VARCHAR(100) NOT NULL, 
	date_of_joining DATETIME DEFAULT NOW(), 
	PRIMARY KEY (id) );
```
