## Setting Up & Using ptunnel-ng
#### Cloning Ptunnel-ng

```shell
3kjS@htb[/htb]$ git clone https://github.com/utoni/ptunnel-ng.git
```
#### Building Ptunnel-ng with Autogen.sh

```shell
3kjS@htb[/htb]$ sudo ./autogen.sh
```
#### Alternative approach of building a static binary

```shell
3kjS@htb[/htb]$ sudo apt install automake autoconf -y 
3kjS@htb[/htb]$ cd ptunnel-ng/ 
3kjS@htb[/htb]$ sed -i '$s/.*/LDFLAGS=-static "${NEW_WD}\/configure" --enable-static $@ \&\& make clean \&\& make -j${BUILDJOBS:-4} all/' autogen.sh 
3kjS@htb[/htb]$ ./autogen.sh
```
#### Transferring Ptunnel-ng to the Pivot Host

```shell
3kjS@htb[/htb]$ scp -r ptunnel-ng ubuntu@10.129.202.64:~/
```
#### Starting the ptunnel-ng Server on the Target Host

```shell
ubuntu@WEB01:~/ptunnel-ng/src$ sudo ./ptunnel-ng -r10.129.202.64 -R22 

[sudo] password for ubuntu: 
./ptunnel-ng: /lib/x86_64-linux-gnu/libselinux.so.1: no version information available (required by ./ptunnel-ng) 
[inf]: Starting ptunnel-ng 1.42. 
[inf]: (c) 2004-2011 Daniel Stoedle, <daniels@cs.uit.no> 
[inf]: (c) 2017-2019 Toni Uhlig, <matzeton@googlemail.com> 
[inf]: Security features by Sebastien Raveau, <sebastien.raveau@epita.fr> 
[inf]: Forwarding incoming ping packets over TCP. 
[inf]: Ping proxy is listening in privileged mode. 
[inf]: Dropping privileges now.
```
#### Connecting to ptunnel-ng Server from Attack Host

```shell
3kjS@htb[/htb]$ sudo ./ptunnel-ng -p10.129.202.64 -l2222 -r10.129.202.64 -R22
```
#### Tunneling an SSH connection through an ICMP Tunnel

```shell
3kjS@htb[/htb]$ ssh -p2222 -lubuntu 127.0.0.1
```
#### Enabling Dynamic Port Forwarding over SSH

```shell
3kjS@htb[/htb]$ ssh -D 9050 -p2222 -lubuntu 127.0.0.1
```
#### Proxychaining through the ICMP Tunnel

```shell
3kjS@htb[/htb]$ proxychains nmap -sV -sT 172.16.5.19 -p3389
```
## Network Traffic Analysis Considerations
![[Pasted image 20260520212256.png]]
