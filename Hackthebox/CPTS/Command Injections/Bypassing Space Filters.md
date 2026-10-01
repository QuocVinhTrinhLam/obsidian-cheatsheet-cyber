## Bypass Blacklisted Operators
![](Pasted%20image%2020260717142119.png)
## Bypass Blacklisted Spaces
![](Pasted%20image%2020260717142711.png)
![](Pasted%20image%2020260717142939.png)
#### Using Tabs
Using tabs (%09) instead of spaces is a technique that may work.So, let us try to use a tab instead of the space character (`127.0.0.1%0a%09`) and see if our request is accepted:
![](Pasted%20image%2020260717142954.png)
#### Using $IFS
Let us use `${IFS}` and see if it works (`127.0.0.1%0a${IFS}`):
![](Pasted%20image%2020260717143026.png)
#### Using Brace Expansion

```bash
3kjS@htb[/htb]$ {ls,-la} 

total 0 
drwxr-xr-x 1 21y4d 21y4d 0 Jul 13 07:37 . 
drwxr-xr-x 1 21y4d 21y4d 0 Jul 13 13:01 ..
```
We can utilize the same method in command injection filter bypasses, by using brace expansion on our command arguments, like (`127.0.0.1%0a{ls,-la}`). To discover more space filter bypasses, check out the [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection#bypass-without-space) page on writing commands without spaces.
