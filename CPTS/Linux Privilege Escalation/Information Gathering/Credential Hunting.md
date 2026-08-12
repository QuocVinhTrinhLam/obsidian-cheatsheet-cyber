```shell
htb_student@NIX02:~$ grep 'DB_USER\|DB_PASSWORD' wp-config.php 

define( 'DB_USER', 'wordpressuser' ); 
define( 'DB_PASSWORD', 'WPadmin123!' );
```

```shell
htb_student@NIX02:~$ find / ! -path "*/proc/*" -iname "*config*" -type f 2>/dev/null
```
## SSH Keys

```shell
htb_student@NIX02:~$ ls ~/.ssh 

id_rsa id_rsa.pub known_hosts
```
