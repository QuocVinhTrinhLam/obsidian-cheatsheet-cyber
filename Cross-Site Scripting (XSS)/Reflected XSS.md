We can try adding any `test` string to see how it's handled:
![](Pasted%20image%2020260626115747.png)

We get `Task 'test' could not be added.`We can try the same XSS payload we used in the previous section and click `Add`:
![To-Do List input with script tag 'alert(window.origin)</script>' and 'Add' button.](https://cdn.services-k8s.prod.aws.htb.systems/content/modules/103/xss_reflected_2.jpg)
![](Pasted%20image%2020260626115812.png)

We can once again view the page source to confirm that the error message includes our XSS payload:

```html
<div></div><ul class="list-unstyled" id="todo"><div style="padding-left:25px">Task '<script>alert(window.origin)</script>' could not be added.</div></ul>
```

As we can see, the single quotes indeed contain our XSS payload `'<script>alert(window.origin)</script>'`.

But if the XSS vulnerability is Non-Persistent, how would we target victims with it?
![](Pasted%20image%2020260626115848.png)
![](Pasted%20image%2020260626115908.png)
