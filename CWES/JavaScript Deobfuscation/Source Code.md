## HTML

![](Source%20Code-20260909-172336.png)

As we can see, the website says `Secret Serial Generator`, We can do that by pressing `[CTRL + U]`, which should open the source view of the website:

![](Source%20Code-20260909-172407.png)
## CSS

```html
<style>
        *,
        html {
            margin: 0;
            padding: 0;
            border: 0;
        }
        ...SNIP...
        h1 {
            font-size: 144px;
        }
        p {
            font-size: 64px;
        }
    </style>
```

If a page `CSS` style is externally defined, the external `.css` file is referred to with the `<link>` tag within the HTML head, as follows:

```html
<head>
    <link rel="stylesheet" href="style.css">
</head>
```
## JavaScript

We can see in our `HTML` source that the `.js` file is referenced externally:

```html
<script src="secret.js"></script>
```

We can check out the script by clicking on `secret.js`, which should take us directly into the script. When we visit it, we see that the code is very complicated and cannot be comprehended:

```js
eval(function (p, a, c, k, e, d) { e = function (c) { '...SNIP... |true|function'.split('|'), 0, {}))
```
