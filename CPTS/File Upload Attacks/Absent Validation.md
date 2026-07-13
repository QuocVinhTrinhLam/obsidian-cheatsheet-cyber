## Arbitrary File Upload
![](Pasted%20image%2020260708130748.png)
![](Pasted%20image%2020260708130752.png)
## Identifying Web Framework

One easy method to determine what language runs the web application is to visit the `/index.ext` page, where we would swap out `ext` with various common web extensions, like `php`, `asp`, `aspx`, among others, to see whether any of them exist.
![](Pasted%20image%2020260708131107.png)
## Vulnerability Identification

To do so, we will write `<?php echo "Hello HTB";?>` to `test.php`, and try uploading it to the web application:
![](Pasted%20image%2020260708131201.png)
![](Pasted%20image%2020260708131211.png)
