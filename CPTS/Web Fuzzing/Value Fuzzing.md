## Custom Wordlist

```shell
3kjS@htb[/htb]$ for i in $(seq 1 1000); do echo $i >> ids.txt; done
```

```shell
3kjS@htb[/htb]$ cat ids.txt 
1 
2 
3 
4 
5 
6 
<...SNIP...>
```
## Value Fuzzing

```shell
3kjS@htb[/htb]$ ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```
