## Parameter Modification

Let us take a look at our target web application. This time, we are provided with credentials for the user `htb-stdnt`. After logging in, we are redirected to `/admin.php?user_id=183`:

![](Authentication%20Bypass%20via%20Parameter%20Modification-20260914-095118.png)

In our web browser, we can see that we seem to be lacking privileges, as we can only see a part of the available data:

![](Authentication%20Bypass%20via%20Parameter%20Modification-20260914-095129.png)

To investigate the purpose of the `user_id` parameter, let us remove it from our request to `/admin.php`. When doing so, we are redirected back to the login screen at `/index.php`, even though our session provided in the `PHPSESSID` cookie is still valid:

![](Authentication%20Bypass%20via%20Parameter%20Modification-20260914-095143.png)

Thus, we can assume that the parameter `user_id` is related to authentication. We can bypass authentication entirely by accessing the URL `/admin.php?user_id=183` directly:

![](Authentication%20Bypass%20via%20Parameter%20Modification-20260914-095200.png)

