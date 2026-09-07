Let's take a look at the google.com page source. Right-click and choose `View Page Source`, and a new tab will open in our browser with the URL `view-source:https://www.google.com/`. Here we can see the `HTML`, `JavaScript`, and external links. Take a moment to browse the page source a bit.

```url
view-source:https://www.google.com/
```
![](Sensitive%20Data%20Exposure-20260907-160416.png)
#### Example

At first glance, this login form does not look like anything out of the ordinary:

![](Sensitive%20Data%20Exposure-20260907-160440.png)

Let's take a look at at the page source:

```html
<form action="action_page.php" method="post">

    <div class="container">
        <label for="uname"><b>Username</b></label>
        <input type="text" required>

        <label for="psw"><b>Password</b></label>
        <input type="password" required>

        <!-- TODO: remove test credentials test:test -->

        <button type="submit">Login</button>
    </div>
</form>

</html>
```

We see that the developers added some comments that they forgot to remove, which contain test credentials:

```html
<!-- TODO: remove test credentials test:test -->
```
