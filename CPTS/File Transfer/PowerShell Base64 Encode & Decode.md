## Encode File Using PowerShell

```Powershell
PS C:\htb> [Convert]::ToBase64String((Get-Content -path "C:\Windows\system32\drivers\etc\hosts" -Encoding byte)) 

<HASH> = PS C:\htb> Get-FileHash "C:\Windows\system32\drivers\etc\hosts" -Algorithm MD5 | select Hash 

Hash 
---- 
3688374325B992DEF12793500307566D
```
## Decode Base64 String in Linux

```Shell
3kjS@htb[/htb]$ echo <HASH> = | base64 -d > hosts
```

```Shell
3kjS@htb[/htb]$ md5sum hosts 3688374325b992def12793500307566d hosts
```

