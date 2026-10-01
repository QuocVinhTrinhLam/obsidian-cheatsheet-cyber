#### ldapsearch

```shell
3kjS@htb[/htb]$ ldapsearch -H ldap://ldap.example.com:389 -D "cn=admin,dc=example,dc=com" -w secret123 -b "ou=people,dc=example,dc=com" "(mail=john.doe@example.com)"
```

This command can be broken down as follows:

- Connect to the server `ldap.example.com` on port `389`.
- Bind (authenticate) as `cn=admin,dc=example,dc=com` with password `secret123`.
- Search under the base DN `ou=people,dc=example,dc=com`.
- Use the filter `(mail=john.doe@example.com)` to find entries that have this email address.
## LDAP Injection
|Input|Description|
|---|---|
|`*`|An asterisk `*` can `match any number of characters`.|
|`( )`|Parentheses `( )` can `group expressions`.|
|`\|`|A vertical bar `\|` can perform `logical OR`.|
|`&`|An ampersand `&` can perform `logical AND`.|
|`(cn=*)`|Input values that try to bypass authentication or authorisation checks by injecting conditions that `always evaluate to true` can be used. For example, `(cn=*)` or `(objectClass=*)` can be used as input values for a username or password fields.|
For example, suppose an application uses the following LDAP query to authenticate users:

```php
(&(objectClass=user)(sAMAccountName=$username)(userPassword=$password))
```

```php
$username = "*"; 
$password = "dummy"; 
(&(objectClass=user)(sAMAccountName=$username)(userPassword=$password))
```

```php
$username = "dummy"; 
$password = "*"; 
(&(objectClass=user)(sAMAccountName=$username)(userPassword=$password))
```
## Enumeration
#### nmap

```shell
3kjS@htb[/htb]$ nmap -p- -sC -sV --open --min-rate=1000 10.129.204.229
```
![](Screenshot%202026-08-02%20at%2021.37.04.png)
