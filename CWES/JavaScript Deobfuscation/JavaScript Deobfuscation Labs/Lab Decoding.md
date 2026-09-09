# [Decoding](Decoding.md)
### Using what you learned in this section, determine the type of encoding used in the string you got at previous exercise, and decode it. To get the flag, you can send a 'POST' request to 'serial.php', and set the data as "serial=YOUR_DECODED_OUTPUT".

```sh
echo 'N2gxNV8xNV9hX3MzY3IzN19tMzU1NGcz' | base64 -d
```
![](Screenshot%202026-09-09%20at%2020.24.26.png)

```sh
curl -X POST http://154.57.164.70:32020/serial.php -d "serial=7h15_15_a_s3cr37_m3554g3"
```
![](Screenshot%202026-09-09%20at%2020.24.44.png)