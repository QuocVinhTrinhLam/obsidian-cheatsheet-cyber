## Uncontrolled Resource Consumption

The endpoint we will be practicing against is vulnerable to [CWE-400: Uncontrolled Resource Consumption](https://cwe.mitre.org/data/definitions/400.html).
### Scenario

![](Unrestricted%20Resource%20Consumption-20260915-143401.png)

![](Unrestricted%20Resource%20Consumption-20260915-143415.png)

Let us attempt to upload a large PDF file containing random bytes. First, we will use `/api/v1/supplier-companies/current-user` to get the supplier-company ID of the currently authenticated user, `b75a7c76-e149-4ca7-9c55-d9fc4ffa87be`:

![](Unrestricted%20Resource%20Consumption-20260915-144422.png)

Next, we will use [dd](https://man7.org/linux/man-pages/man1/dd.1.html) to create a file containing 30 random megabytes and assign it the `.pdf` extension:

```sh
3kjS@htb[/htb]$ dd if=/dev/urandom of=certificateOfIncorporation.pdf bs=1M count=30

30+0 records in
30+0 records out
31457280 bytes (31 MB, 30 MiB) copied, 0.139503 s, 225 MB/s
```

![](Unrestricted%20Resource%20Consumption-20260915-144451.png)

After invoking the endpoint, we notice that the API returns a successful upload message, along with the size of the uploaded file:

![](Unrestricted%20Resource%20Consumption-20260915-144510.png)

```sh
3kjS@htb[/htb]$ dd if=/dev/urandom of=reverse-shell.exe bs=1M count=10

10+0 records in
10+0 records out
10485760 bytes (10 MB, 10 MiB) copied, 0.0398348 s, 263 MB/s
```

Within the `/api/v1/supplier-companies/certificates-of-incorporation` `POST` endpoint, we will click on the 'Choose File' button and upload the file:

![](Unrestricted%20Resource%20Consumption-20260915-144601.png)

![](Unrestricted%20Resource%20Consumption-20260915-144606.png)
### Abusing Default Behaviors

```sh
3kjS@htb[/htb]$ curl -O http://94.237.51.179:51135/SupplierCompaniesCertificatesOfIncorporations/reverse-shell.exe
```
