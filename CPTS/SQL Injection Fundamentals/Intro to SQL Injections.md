## Use of SQL in Web Applications

```php
$conn = new mysqli("localhost", "root", "password", "users"); 
$query = "select * from logins"; 
$result = $conn->query($query);
```

```php
while($row = $result->fetch_assoc() ){ echo $row["name"]."<br>"; }
```

```php
$searchInput = $_POST['findUser']; 
$query = "select * from logins where username like '%$searchInput'"; 
$result = $conn->query($query);
```
## SQL Injection

```php
$searchInput = $_POST['findUser']; 
$query = "select * from logins where username like '%$searchInput'"; 
$result = $conn->query($query);
```

```sql
select * from logins where username like '%$searchInput'
```

```php
'%1'; DROP TABLE users;'
```

`Notice how we added a single quote (') after "1", in order to escape the bounds of the user-input in ('%$searchInput').`

```sql
select * from logins where username like '%1'; DROP TABLE users;'
```
## Syntax Errors

```php
Error: near line 1: near "'": syntax error
```

```sql
select * from logins where username like '%1'; DROP TABLE users;'
```
## Types of SQL Injections
![](Pasted%20image%2020260615212733.png)
