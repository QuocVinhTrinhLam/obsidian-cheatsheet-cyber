# [Attacking Splunk](Attacking%20Splunk.md)
### Attack the Splunk target and gain remote code execution. Submit the contents of the flag.txt file in the c:\loot directory.
![](Screenshot%202026-07-30%20at%2013.23.27.png)

`run.ps1`
![](Screenshot%202026-07-30%20at%2013.28.26.png)

```shell
tar -cvzf updater.tar.gz reverse_shell_splunk/
```

![](Screenshot%202026-07-30%20at%2013.32.03.png)
But i need to rm the `updater.tar.gz`

Initiate netcat listener
![](Screenshot%202026-07-30%20at%2013.33.04.png)
![](Screenshot%202026-07-30%20at%2013.32.52.png)![](Screenshot%202026-07-30%20at%2013.39.17.png)
![](Screenshot%202026-07-30%20at%2013.39.54.png)
