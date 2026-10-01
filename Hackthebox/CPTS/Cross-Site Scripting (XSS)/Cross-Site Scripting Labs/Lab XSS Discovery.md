# [XSS Discovery](XSS%20Discovery.md)
### Utilize some of the techniques mentioned in this section to identify the vulnerable input parameter found in the above server. What is the name of the vulnerable parameter?

```shell
python3 xsstrike.py -u 'http://154.57.164.68:32049/?fullname=Hacker&username=Ethical&password=123%40&email=ethheck%4gmail.com'
```
![](Screenshot%202026-06-26%20at%2013.04.34.png)

`email`
### What type of XSS was found on the above server? "name only"

`reflected`