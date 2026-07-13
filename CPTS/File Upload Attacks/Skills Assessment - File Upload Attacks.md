### Try to exploit the upload form to read the flag found at the root directory "/".

#### Preparing for Filter Evasion
![](Screenshot%202026-07-13%20at%2011.46.29.png)
![](Screenshot%202026-07-13%20at%2011.46.12.png)
#### Content-Type Filter Bypass
![](Screenshot%202026-07-13%20at%2011.48.08.png)
![](Pasted%20image%2020260713120101.png)
#### Exploiting XXE via SVG Payload
![](Screenshot%202026-07-13%20at%2012.00.24.png)

Then i decoded
![](Screenshot%202026-07-13%20at%2012.04.33.png)
#### Achieving Remote Code Execution (RCE)

Hex: FF D8 FF E0
![](Screenshot%202026-07-13%20at%2012.18.25.png)
![](Screenshot%202026-07-13%20at%2012.19.28.png)

The format file restored is (YMD_)
![](Screenshot%202026-07-13%20at%2012.24.42.png)
![](Screenshot%202026-07-13%20at%2012.23.56.png)

Used directory traversal (`cd ../../../..`) to reach the system root
![](Screenshot%202026-07-13%20at%2012.28.22.png)
![](Screenshot%202026-07-13%20at%2012.29.08.png)
