![](Pasted%20image%2020260802120001.png)
## Retrieving hardcoded Credentials from Thick-Client Applications

```cmd
C:\Apps>.\Restart-OracleService.exe 
C:\Apps>
```
![](Pasted%20image%2020260802121713.png)

In order to capture the files, it is required to change the permissions of the `Temp` folder to disallow file deletions. To do this, we right-click the folder `C:\Users\Matt\AppData\Local\Temp` and under `Properties` -> `Security` -> `Advanced` -> `cybervaca` -> `Disable inheritance` -> `Convert inherited permissions into explicit permissions on this object` -> `Edit` -> `Show advanced permissions`, we deselect the `Delete subfolders and files`, and `Delete` checkboxes.
![](Pasted%20image%2020260802121720.png)
![](Screenshot%202026-08-02%20at%2012.18.09.png)

Listing the content of the `6F39` batch file reveals the following.
![](Screenshot%202026-08-02%20at%2012.18.38.png)
![](Screenshot%202026-08-02%20at%2012.19.06.png)

```powershell
C:\> cat C:\programdata\monta.ps1

$salida = $null; $fichero = (Get-Content C:\ProgramData\oracle.txt) ; foreach ($linea in $fichero) {$salida += $linea }; $salida = $salida.Replace(" ",""); [System.IO.File]::WriteAllBytes("c:\programdata\restart-service.exe", [System.Convert]::FromBase64String($salida))
```

Now when executing `restart-service.exe` we are presented with the banner `Restart Oracle` created by `HelpDesk` back in 2010.

```powershell
C:\> .\restart-service.exe
```
![](Screenshot%202026-08-02%20at%2012.19.59.png)
![](Pasted%20image%2020260802122005.png)

Let's start `x64dbg`, navigate to `Options` -> `Preferences`, and uncheck everything except `Exit Breakpoint`:
![](Pasted%20image%2020260802122010.png)
![](Pasted%20image%2020260802122021.png)
![](Pasted%20image%2020260802122024.png)
![](Pasted%20image%2020260802122033.png)

```powershell
C:\> C:\TOOLS\Strings\strings64.exe .\restart-service_00000000001E0000.bin 

<SNIP> 
"#M 
z\V 
).NETFramework,Version=v4.0,Profile=Client 
FrameworkDisplayName 
.NET Framework 4 Client Profile 
<SNIP>
```
![](Screenshot%202026-08-02%20at%2012.21.18.png)
![](Pasted%20image%2020260802122124.png)
