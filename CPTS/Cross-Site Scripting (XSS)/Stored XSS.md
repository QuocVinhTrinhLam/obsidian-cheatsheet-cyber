## XSS Testing Payloads

```html
<script>alert(window.origin)</script>
```
![](Pasted%20image%2020260626114603.png)

We can confirm this further by looking at the page source by clicking `CTRL+U` or right-clicking and selecting `View Page Source`, and we should see our payload in the page source:

```html
<div></div><ul class="list-unstyled" id="todo"><ul><script>alert(window.origin)
</script> 
</ul></ul>
```
