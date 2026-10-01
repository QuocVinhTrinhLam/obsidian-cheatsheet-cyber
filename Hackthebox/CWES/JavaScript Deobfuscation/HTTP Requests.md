## cURL

```sh
3kjS@htb[/htb]$ curl http://SERVER_IP:PORT/

</html>
<!DOCTYPE html>

<head>
    <title>Secret Serial Generator</title>
    <style>
        *,
        html {
            margin: 0;
            padding: 0;
            border: 0;
...SNIP...
        <h1>Secret Serial Generator</h1>
        <p>This page generates secret serials!</p>
    </div>
</body>

</html>
```
## POST Request

```sh
3kjS@htb[/htb]$ curl -s http://SERVER_IP:PORT/ -X POST
```

```sh
3kjS@htb[/htb]$ curl -s http://SERVER_IP:PORT/ -X POST -d "param1=sample"
```
