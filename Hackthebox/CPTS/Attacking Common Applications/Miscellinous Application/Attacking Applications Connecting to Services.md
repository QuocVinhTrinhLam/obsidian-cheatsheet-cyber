## ELF Executable Examination

```shell
3kjS@htb[/htb]$ ./octopus_checker
```

The binary probably connects using a SQL connection string that contains credentials. Using tools like [PEDA](https://github.com/longld/peda) (Python Exploit Development Assistance for GDB) we can further examine the file.

```shell
3kjS@htb[/htb]$ gdb ./octopus_checker
```

```assembly
gdb-peda$ set disassembly-flavor intel 
gdb-peda$ disas main
```
![](Screenshot%202026-08-03%20at%2012.27.07.png)

Further down the function, we see a call to SQLDriverConnect.
![](Screenshot%202026-08-03%20at%2012.29.16.png)

Adding a breakpoint at this address and running the program once again, reveals a SQL connection string in the RDX register address, containing the credentials for a local database instance.
![](Screenshot%202026-08-03%20at%2012.29.33.png)
## DLL File Examination

```powershell
C:\> Get-FileMetaData .\MultimasterAPI.dll
```
![](Screenshot%202026-08-03%20at%2012.29.57.png)
![](Attacking%20Applications%20Connecting%20to%20Services-20260803-123008.png)
