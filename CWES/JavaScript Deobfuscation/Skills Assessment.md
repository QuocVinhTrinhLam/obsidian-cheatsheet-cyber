### Try to study the HTML code of the webpage, and identify used JavaScript code within it. What is the name of the JavaScript file being used?

```sh
curl 154.57.164.82:32005
```
![](Screenshot%202026-09-09%20at%2020.29.59.png)
### Once you find the JavaScript code, try to run it to see if it does any interesting functions. Did you get something in return?

```url
view-source:http://154.57.164.82:32005/api.min.js
```
![](Screenshot%202026-09-09%20at%2020.40.13.png)
### As you may have noticed, the JavaScript code is obfuscated. Try applying the skills you learned in this module to deobfuscate the code, and retrieve the 'flag' variable.


![](Screenshot%202026-09-09%20at%2020.42.52.png)
### Try to Analyze the deobfuscated JavaScript code, and understand its main functionality. Once you do, try to replicate what it's doing to get a secret key. What is the key?

We see that the endpoint `/keys.php`

![](Screenshot%202026-09-09%20at%2020.53.06.png)

```sh
curl 154.57.164.82:32005/keys.php -X POST

4150495f70336e5f37333537316e365f31355f66756e
```
### Once you have the secret key, try to decide it's encoding method, and decode it. Then send a 'POST' request to the same previous page with the decoded key as "key=DECODED_KEY". What is the flag you got?

Try to decode that value

```sh
echo '4150495f70336e5f37333537316e365f31355f66756e' | xxd -p -r 

API_p3n_73571n6_15_fun
```

Now get the flag easily 

```sh
curl 154.57.164.82:32005/keys.php -X POST -d "key=API_p3n_73571n6_15_fun"

HTB{r34dy_70_h4ck_my_w4y_1n_2_HTB}
```
