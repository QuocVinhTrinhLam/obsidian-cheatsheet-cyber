This is our site that we need to do

![](Screenshot%202026-09-13%20at%2015.45.48.png)

When the main page loads, several POST requests

![](Screenshot%202026-09-13%20at%2015.58.26.png)

Send to repeater

![](Screenshot%202026-09-13%20at%2016.01.57.png)

Let's test some SSTI [PayloadsAllTheThings SSTI CheatSheet](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md)

![](Screenshot%202026-09-13%20at%2016.06.00.png)

Got this !

![](Screenshot%202026-09-13%20at%2016.07.37.png)

Not a Jinja template

![](Screenshot%202026-09-13%20at%2016.09.25.png)

So, it's Twig template injection

![](Screenshot%202026-09-13%20at%2016.10.04.png)

![](Screenshot%202026-09-13%20at%2016.14.39.png)

{IFS} variable as a substitute for the space character

![](Screenshot%202026-09-13%20at%2016.16.59.png)

So our command to get the flag is:

```
{{['cat${IFS}/flag.txt']|filter('system')}}
```

![](Screenshot%202026-09-13%20at%2016.18.05.png)