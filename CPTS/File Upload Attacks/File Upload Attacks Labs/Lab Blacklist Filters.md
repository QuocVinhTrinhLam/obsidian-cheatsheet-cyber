# [Blacklist Filters](Blacklist%20Filters.md)
### Try to find an extension that is not blacklisted and can execute PHP code on the web server, and use it to read "/flag.txt"

First of all, i need to prepare a `test.jpg` within `<?php echo "HELLO_WORLD_EXECUTED"; ?>`
![](Screenshot%202026-07-08%20at%2015.44.08.png)

I've intercepted then sent it to intruder for brute forcing the web extensions, i got `phar`
![](Screenshot%202026-07-08%20at%2015.42.53.png)

Then i uploaded on server and got a shell
![](Screenshot%202026-07-08%20at%2015.44.38.png)