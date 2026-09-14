## Direct Access

```php
if(!$_SESSION['active']) {
    header("Location: index.php");
}
```

This code redirects the user to `/index.php` if the session is not active, i.e., if the user is not authenticated. However, the PHP script does not stop execution, resulting in protected information within the page being sent in the response body:

![](Authentication%20Bypass%20via%20Direct%20Access-20260914-094108.png)

Afterward, browse to the `/admin.php` endpoint in the web browser. Next, right-click on the request and select `Do intercept > Response to this request` to intercept the response:

![](Authentication%20Bypass%20via%20Direct%20Access-20260914-094130.png)

![](Authentication%20Bypass%20via%20Direct%20Access-20260914-094137.png)

![](Authentication%20Bypass%20via%20Direct%20Access-20260914-094144.png)

To prevent the protected information from being returned in the body of the redirect response, the PHP script needs to exit after issuing the redirect:

```php
if(!$_SESSION['active']) {
    header("Location: index.php");
    exit;
}
```
