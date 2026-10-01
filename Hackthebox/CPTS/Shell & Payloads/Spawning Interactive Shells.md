## /bin/sh -i

```shell
/bin/sh -i 
sh: no job control in this shell 
sh-4.2$
```
## Perl

```shell
perl -e 'exec "/bin/sh";'
```

```shell
perl: exec "/bin/sh";
```
## Ruby

#### Ruby To Shell

```shell
ruby: exec "/bin/sh"
```
## Lua
#### Lua To Shell

```shell
lua: os.execute('/bin/sh')
```
## AWK
#### AWK To Shell

```shell
awk 'BEGIN {system("/bin/sh")}'
```
## Find
#### Using Find For A Shell

```shell
find / -name nameoffile -exec /bin/awk 'BEGIN {system("/bin/sh")}' \;
```
## Using Exec To Launch A Shell

```shell
find . -exec /bin/sh \; -quit
```
## VIM
#### Vim To Shell

```shell
vim -c ':!/bin/sh'
```
#### Vim Escape

```shell
vim 
:set shell=/bin/sh 
:shell
```
## Execution Permissions Considerations
#### Permissions

```shell
ls -la <path/to/fileorbinary>
```
#### Sudo -l

```shell
sudo -l 
Matching Defaults entries for apache on ILF-WebSrv: 
env_reset, mail_badpass,
secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User apache may run the following commands on ILF-WebSrv: 
	(ALL : ALL) NOPASSWD: ALL
```

