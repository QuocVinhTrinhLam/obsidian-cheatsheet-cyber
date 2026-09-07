# [Internal Information Gathering](Internal%20Information%20Gathering.md)
### Mount an NFS share and find a flag.txt file. Submit the contents as your answer.

```shell
ssh -D 8081 -i prv_key root@10.129.229.147
```
![](Screenshot%202026-09-02%20at%2021.02.15.png)

```shell
for i in $(seq 254); do ping 172.16.8.$i -c1 -W1 & done | grep from
```
![](Screenshot%202026-09-02%20at%2021.03.06.png)

```shell
proxychains showmount -e 172.16.8.20
```
![](Screenshot%202026-09-02%20at%2021.06.30.png)

```shell
mount -t nfs 172.16.8.20:/DEV01 /tmp/DEV01
```
![](Screenshot%202026-09-02%20at%2021.10.16.png)