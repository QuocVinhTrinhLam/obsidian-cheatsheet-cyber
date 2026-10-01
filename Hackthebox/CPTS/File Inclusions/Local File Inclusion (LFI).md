## Basic LFI
![](Pasted%20image%2020260629142046.png)
![](Screenshot%202026-06-29%20at%2014.20.52.png)
![](Screenshot%202026-06-29%20at%2014.20.58.png)
## Path Traversal

```php
include("./languages/" . $_GET['language']);
```
![](Screenshot%202026-06-29%20at%2014.23.26.png)
![](Screenshot%202026-06-29%20at%2014.23.38.png)
## Filename Prefix

```php
include("lang_" . $_GET['language']);
```
![](Screenshot%202026-06-29%20at%2014.24.45.png)
![](Screenshot%202026-06-29%20at%2014.25.08.png)
## Appended Extensions

```php
include($_GET['language'] . ".php");
```
![](Screenshot%202026-06-29%20at%2014.25.45.png)
