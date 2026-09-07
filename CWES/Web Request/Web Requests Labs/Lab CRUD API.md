# [CRUD API](CRUD%20API.md)
### First, try to update any city's name to be 'flag'. Then, delete any city. Once done, search for a city named 'flag' to get the flag.

```shell
curl -X PUT http://154.57.164.78:30373/api.php/city/Memphis -d '{"city_name":"flag", "country_name":"HTB"}' -H 'Content-Type: application/json'
```
![](Screenshot%202026-09-07%20at%2014.44.46.png)