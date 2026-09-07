# [POST](POST.md)
### Obtain a session cookie through a valid login, and then use the cookie with cURL to search for the flag through a JSON POST request to '/search.php'

Cookie : d7qgf2phtq1mm7pes0r7u1t729
![](Screenshot%202026-09-07%20at%2014.15.51.png)


![](Screenshot%202026-09-07%20at%2014.16.20.png)

```shell
curl -g -X POST http://154.57.164.78:32588/search.php -H 'Content-Type: application/json' -d '{"search":"flag"}' -b 'PHPSESSID=d7qgf2phtq1mm7pes0r7u1t729'
```
![](Screenshot%202026-09-07%20at%2014.26.27.png)