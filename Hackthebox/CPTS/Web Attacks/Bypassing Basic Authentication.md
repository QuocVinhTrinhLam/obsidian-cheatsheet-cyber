## Identify
![](Pasted%20image%2020260718141430.png)
![](Pasted%20image%2020260718141433.png)

As we do not have any credentials, we will get a `401 Unauthorized` page:
![](Pasted%20image%2020260718141440.png)
## Exploit
![](Pasted%20image%2020260718141500.png)
![](Pasted%20image%2020260718141504.png)
![](Pasted%20image%2020260718141508.png)

```shell
3kjS@htb[/htb]$ curl -i -X OPTIONS http://SERVER_IP:PORT/ 

HTTP/1.1 200 OK 
Date: 
Server: Apache/2.4.41 (Ubuntu) 
Allow: POST,OPTIONS,HEAD,GET 
Content-Length: 0 
Content-Type: httpd/unix-directory
```
![](Pasted%20image%2020260718142126.png)
![](Pasted%20image%2020260718142228.png)
