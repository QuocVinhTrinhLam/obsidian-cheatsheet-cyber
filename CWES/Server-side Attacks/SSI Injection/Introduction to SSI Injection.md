Server-Side Includes (SSI) is a technology that web applications use to create dynamic content on HTML pages. SSI is supported by many popular web servers such as [Apache](https://httpd.apache.org/docs/current/howto/ssi.html) and [IIS](https://learn.microsoft.com/en-us/iis/configuration/system.webserver/serversideinclude). The use of SSI can often be inferred from the file extension. Typical file extensions include `.shtml`, `.shtm`, and `.stm`. However, web servers can be configured to support SSI directives in arbitrary file extensions. As such, we cannot conclusively determine whether SSI is used solely based on the file extension.
## SSI Directives

An SSI directive has the following syntax:

```ssi
<!--#name param1="value1" param2="value" -->
```
#### printenv

```ssi
<!--#printenv -->
```
#### config

```ssi
<!--#config errmsg="Error!" -->
```
#### echo

```ssi
<!--#echo var="DOCUMENT_NAME" var="DATE_LOCAL" -->
```
#### exec

```ssi
<!--#exec cmd="whoami" -->
```
#### include

```ssi
<!--#include virtual="index.html" -->
```
