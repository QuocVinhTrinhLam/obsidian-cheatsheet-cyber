
```shell
htb_student@NIX02:~$ echo $PATH 

/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
```

Creating a script or program in a directory specified in the PATH will make it executable from any directory on the system.

```shell
htb_student@NIX02:~$ pwd && conncheck
```
![](Screenshot%202026-08-04%20at%2013.09.44.png)

As shown below, the `conncheck` script created in `/usr/local/sbin` will still run when in the `/tmp` directory because it was created in a directory specified in the PATH.

```shell
htb_student@NIX02:~$ pwd && conncheck
```
![](Screenshot%202026-08-04%20at%2013.10.05.png)

```shell
htb_student@NIX02:~$ echo $PATH

/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
```

```shell
htb_student@NIX02:~$ PATH=.:${PATH} 
htb_student@NIX02:~$ export PATH 
htb_student@NIX02:~$ echo $PATH 

.:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
```

In this example, we modify the path to run a simple `echo` command when the command `ls` is typed.

```shell
htb_student@NIX02:~$ touch ls 
htb_student@NIX02:~$ echo 'echo "PATH ABUSE!!"' > ls 
htb_student@NIX02:~$ chmod +x ls
```

```shell
htb_student@NIX02:~$ ls 

PATH ABUSE!!
```
