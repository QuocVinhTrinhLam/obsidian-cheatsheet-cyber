### Run a sub-domain/vhost fuzzing scan on '*.academy.htb' for the IP shown above. What are all the sub-domains you can identify? (Only write the sub-domain name)

```shell
ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:31681 -H 'Host:FUZZ.academy.htb' -fs 985
```
![](Screenshot%202026-06-12%20at%2012.11.09.png)
### Before you run your page fuzzing scan, you should first run an extension fuzzing scan. What are the different extensions accepted by the domains?

```shell
ffuf -w /opt/useful/seclists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://faculty.academy.htb:31681//indexFUZZ
```
![](Screenshot%202026-06-12%20at%2012.14.50.png)
### One of the pages you will identify should say 'You don't have access!'. What is the full page URL?

```shell
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:FUZZ -u http://faculty.academy.htb:31681/courses/FUZZ -e .php,.phps,.php7 -recursion -recursion-depth 1 -fs 287
```
![](Screenshot%202026-06-12%20at%2012.29.38.png)
![](Screenshot%202026-06-12%20at%2012.30.23.png)
### In the page from the previous question, you should be able to find multiple parameters that are accepted by the page. What are they?

```shell
ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://faculty.academy.htb:31681/courses/linux-security.php7 -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs 774
```
![](Screenshot%202026-06-12%20at%2012.38.08.png)
### Try fuzzing the parameters you identified for working values. One of them should return a flag. What is the content of the flag?

```shell
ffuf -w /opt/useful/seclists/Usernames/xato-net-10-millions-usernames.txt -u http://faculty.academy.htb:31681/courses/linux-security.php7 -X POST -d 'username=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded'
```
![](Screenshot%202026-06-12%20at%2012.47.44.png)

```shell
curl http://faculty.academy.htb:31681/courses/linux-security.php7 -X POST -d 'username=harry' -H 'Content-Type: application/x-www-form-urlencoded'
```
![](Screenshot%202026-06-12%20at%2012.49.06.png)
