# [Validating Findings](Validating%20Findings.md)
### Fuzz the target system using DirBuster-2007_directory-list-2.3-medium.txt, looking for a hidden directory. Once you have found the hidden directory, responsibly determine the validity of the vulnerability by analyzing the tar.gz file in the directory. Answer using the full Content-Length header, eg "Content-Length: 1337"

```sh
feroxbuster -u http://154.57.164.77:31554 -w /usr/share/seclists/Discovery/Web-Content/DirBuster-2007_directory-list-2.3-medium.txt -x .gz -t 300
```
![](Screenshot%202026-09-09%20at%2013.42.58.png)

```sh
curl -I http://154.57.164.77:31554/ur-hiddenmember/backup.tar.gz
```
![](Screenshot%202026-09-09%20at%2013.43.26.png)
