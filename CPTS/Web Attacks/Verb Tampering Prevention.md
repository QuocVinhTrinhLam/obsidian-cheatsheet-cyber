## Insecure Configuration

```xml
<Directory "/var/www/html/admin"> 
	AuthType Basic 
	AuthName "Admin Panel" 
	AuthUserFile /etc/apache2/.htpasswd 
	<Limit GET> 
		Require valid-user 
	</Limit> 
</Directory>
```

Finally, the following is an example for an `ASP.NET` configuration found in the `web.config` file of a web application:

```xml
<system.web> 
	<authorization> 
		<allow verbs="GET" roles="admin"> 
			<deny verbs="GET" users="*"> 
		</deny> 
		</allow> 
	</authorization> 
</system.web>
```
## Insecure Coding

```php
if (isset($_REQUEST['filename'])) { 
	if (!preg_match('/[^A-Za-z0-9. _-]/', $_POST['filename'])) { 
		system("touch " . $_REQUEST['filename']); 
	} else { 
		echo "Malicious Request Denied!"; 
	} 
}
```

|Language|Function|
|---|---|
|PHP|`$_REQUEST['param']`|
|Java|`request.getParameter('param')`|
|C#|`Request['param']`|
