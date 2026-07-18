## Command Injection Detection
![](Pasted%20image%2020260717135038.png)
![](Pasted%20image%2020260717135042.png)

```bash
ping -c 1 OUR_INPUT
```
## Command Injection Methods
|**Injection Operator**|**Injection Character**|**URL-Encoded Character**|**Executed Command**|
|---|---|---|---|
|Semicolon|`;`|`%3b`|Both|
|New Line|`\n`|`%0a`|Both|
|Background|`&`|`%26`|Both (second output generally shown first)|
|Pipe|`\|`|`%7c`|Both (only second output is shown)|
|AND|`&&`|`%26%26`|Both (only if first succeeds)|
|OR|`\|`|`%7c%7c`|Second (only if first fails)|
|Sub-Shell|` `` `|`%60%60`|Both **(Linux-only)**|
|Sub-Shell|`$()`|`%24%28%29`|Both **(Linux-only)**|
