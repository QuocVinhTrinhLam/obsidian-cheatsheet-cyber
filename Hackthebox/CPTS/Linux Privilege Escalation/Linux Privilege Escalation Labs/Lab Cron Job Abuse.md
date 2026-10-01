# [Cron Job Abuse](Cron%20Job%20Abuse.md)
### Connect to the target system and escalate privileges by abusing the misconfigured cron job. Submit the contents of the flag.txt file in the /root/cron_abuse directory.

```shell
./pspy64 -pf -i 1000
```
![](Screenshot%202026-08-09%20at%2013.09.36.png)

```shell
ls -l /dmz-backups/backup.sh
-rwxrwxrwx 1 root root 189 Nov  6  2020 /dmz-backups/backup.sh
```

Modified the backup.sh
![](Screenshot%202026-08-09%20at%2013.11.42.png)

Now i gonna initiate the listener and wait for the cronjob about 3 mins 
```shell
nc -lvnp 8080
```
![](Screenshot%202026-08-09%20at%2013.14.24.png)