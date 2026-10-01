### After completing all steps in the assessment, you will be presented with a page that contains a flag in the format of HTB{...}. What is that flag?

```sh
gobuster dir -u http://154.57.164.65:31882 -w /usr/share/seclists/Discovery/Web-Content/common.txt
```
![](Screenshot%202026-09-09%20at%2014.22.10.png)

```sh
feroxbuster -u http://154.57.164.65:31882 -w /usr/share/seclists/Discovery/Web-Content/common.txt -t 300 -x .php,.html
```
![](Screenshot%202026-09-09%20at%2014.42.58.png)

```sh
curl http://154.57.164.65:31882/admin/panel.php
```
![](Screenshot%202026-09-09%20at%2014.44.02.png)

```sh
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -fc 404 -u "http://154.57.164.65:31882/admin/panel.php?accessID=FUZZ" -fs 58
```
![](Screenshot%202026-09-09%20at%2014.46.03.png)

```sh
curl http://154.57.164.65:31882/admin/panel.php?accessID=getaccess
```
![](Screenshot%202026-09-09%20at%2014.46.43.png)

Added vhost into /etc/hosts
![](Screenshot%202026-09-09%20at%2014.47.47.png)

```sh
gobuster vhost -u http://fuzzing_fun.htb:31882 -w /usr/share/seclists/Discovery/Web-Content/common.txt --append-domain
```
![](Screenshot%202026-09-09%20at%2014.51.39.png)

```sh
curl hidden.fuzzing_fun.htb:31882

curl hidden.fuzzing_fun.htb:31882/godeep
```
![](Screenshot%202026-09-09%20at%2014.54.09.png)

```sh
feroxbuster -u http://hidden.fuzzing_fun.htb:31882/godeep:31882 -w /usr/share/seclists/Discovery/Web-Content/common.txt -t 300 -x .php,.html
```
![](Screenshot%202026-09-09%20at%2014.58.59.png)

```sh
curl http://hidden.fuzzing_fun.htb:31882/godeep/stoneedge/bbclone/typo3/
```
![](Screenshot%202026-09-09%20at%2014.59.19.png)
