
```url
http://gitlab.inlanefreight.local:8081/admin/application_settings/general
```
![](Pasted%20image%2020260801205457.png)

If we can obtain user credentials from our OSINT, we may be able to log in to a GitLab instance. Two-factor authentication is disabled by default.

```url
http://gitlab.inlanefreight.local:8081/admin/application_settings/general
```
![](Pasted%20image%2020260801205515.png)
## Footprinting & Discovery

```url
http://gitlab.inlanefreight.local:8081/users/sign_in
```
![](Pasted%20image%2020260801205549.png)
## Enumeration

```url
http://gitlab.inlanefreight.local:8081/explore
```
![](Pasted%20image%2020260801205720.png)

```url
http://gitlab.inlanefreight.local:8081/root/inlanefreight-dev
```
![](Pasted%20image%2020260801205857.png)

```url
http://gitlab.inlanefreight.local:8081/users/sign_up
```
![](Pasted%20image%2020260801205909.png)

```url
http://gitlab.inlanefreight.local:8081/users/sign_up
```
![](Pasted%20image%2020260801210029.png)
