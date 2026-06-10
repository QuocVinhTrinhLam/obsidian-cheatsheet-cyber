### The /lucky.php page has a button that appears to be disabled. Try to enable the button, and then click it to get the flag.
![](Screenshot%202026-06-10%20at%2010.29.33.png)

### The /admin.php page uses a cookie that has been encoded multiple times. Try to decode the cookie until you get a value with 31-characters. Submit the value as the answer.
![](Screenshot%202026-06-09%20at%2017.30.37.png)
![](Pasted%20image%2020260609165407.png)

### Once you decode the cookie, you will notice that it is only 31 characters long, which appears to be an md5 hash missing its last character. So, try to fuzz the last character of the decoded md5 cookie with all alpha-numeric characters, while encoding each request with the encoding methods you identified above. (You may use the "alphanum-case.txt" wordlist from Seclist for the payload)
![](Screenshot%202026-06-09%20at%2016.53.43.png)
### You are using the 'auxiliary/scanner/http/coldfusion_locale_traversal' tool within Metasploit, but it is not working properly for you. You decide to capture the request sent by Metasploit so you can manually verify it and repeat it. Once you capture the request, what is the 'XXXXX' directory being called in '/XXXXX/administrator/..'?

```shell
msf6 >search coldfusion_locale_traversal 
msf6 >use 0 
msf6 >set RHOSTS 83.136.253.59 
msf6 >set RPORT 59076 
msf6 >set PROXIES HTTP:127.0.0.1:8080 
msf6 >exploit
```
![](Pasted%20image%2020260610103053.png)