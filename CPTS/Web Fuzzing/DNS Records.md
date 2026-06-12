Once we accessed the page under `/blog`, we got a message saying `Admin panel moved to academy.htb`. If we visit the website in our browser, we get `can’t connect to the server at www.academy.htb`:
![](Pasted%20image%2020260610213122.png)

```shell
3kjS@htb[/htb]$ sudo sh -c 'echo "SERVER_IP academy.htb" >> /etc/hosts'
```
