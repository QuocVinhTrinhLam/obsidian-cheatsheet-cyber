## Running JavaScript code

Let us take the following line of code as an example and attempt to obfuscate it:

```js
console.log('HTB JavaScript Deobfuscation Module');
```

First, let us test running this code in cleartext, to see it work in action. We can go to [JSConsole](https://jsconsole.com/), paste the code and hit enter, and see its output:

```url
https://jsconsole.com
```
![](Basic%20Obfuscation-20260909-194128.png)
## Minifying JavaScript code

Many tools can help us minify JavaScript code, like [javascript-minifier](https://javascript-minifier.com/). We simply copy our code, and click `Minify`, and we get the minified output on the right:

```url
https://javascript-minifier.com/
```
![](Basic%20Obfuscation-20260909-194203.png)
## Packing JavaScript code

Now, let us obfuscate our line of code to make it more obscure and difficult to read. First, we will try [BeautifyTools](http://beautifytools.com/javascript-obfuscator.php) to obfuscate our code:

```url
http://beautifytools.com/javascript-obfuscator.php
```
![](Basic%20Obfuscation-20260909-194221.png)

```js
eval(function(p,a,c,k,e,d){e=function(c){return c};if(!''.replace(/^/,String)){while(c--){d[c]=k[c]||c}k=[function(e){return d[e]}];e=function(){return'\\w+'};c=1};while(c--){if(k[c]){p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c])}}return p}('5.4(\'3 2 1 0\');',6,6,'Module|Deobfuscation|JavaScript|HTB|log|console'.split('|'),0,{}))
```

```url
https://jsconsole.com
```
![](Basic%20Obfuscation-20260909-194235.png)
