#### Nmap - Web Discovery

```shell
3kjS@htb[/htb]$ nmap -p 80,443,8000,8080,8180,8888,10000 --open -oA web_discovery -iL scope_list
```
## Initial Enumeration

```shell
3kjS@htb[/htb]$ cat scope_list
app.inlanefreight.local 
dev.inlanefreight.local 
drupal-dev.inlanefreight.local 
drupal-qa.inlanefreight.local 
drupal-acc.inlanefreight.local 
drupal.inlanefreight.local 
blog-dev.inlanefreight.local 
blog.inlanefreight.local 
app-dev.inlanefreight.local 
jenkins-dev.inlanefreight.local 
jenkins.inlanefreight.local 
web01.inlanefreight.local 
gitlab-dev.inlanefreight.local 
gitlab.inlanefreight.local 
support-dev.inlanefreight.local 
support.inlanefreight.local 
inlanefreight.local 
10.129.201.50
```

```shell
3kjS@htb[/htb]$ sudo nmap -p 80,443,8000,8080,8180,8888,10000 --open -oA web_discovery -iL scope_list
```

```shell
3kjS@htb[/htb]$ sudo nmap --open -sV 10.129.201.50
```
## Using EyeWitness

```shell
3kjS@htb[/htb]$ sudo apt install eyewitness
```

```shell
3kjS@htb[/htb]$ eyewitness --web -x web_discovery.xml -d inlanefreight_eyewitness
```
## Using Aquatone

```shell
3kjS@htb[/htb]$ wget https://github.com/michenriksen/aquatone/releases/download/v1.7.0/aquatone_linux_amd64_1.7.0.zip
```

```shell
3kjS@htb[/htb]$ echo $PATH /home/mrb3n/.local/bin:/snap/bin:/usr/sandbox/:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/usr/share/games:/usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
```

```shell
3kjS@htb[/htb]$ cat web_discovery.xml | ./aquatone -nmap
```
## Interpreting the Results
![](Pasted%20image%2020260727113256.png)
![](Pasted%20image%2020260727113356.png)
