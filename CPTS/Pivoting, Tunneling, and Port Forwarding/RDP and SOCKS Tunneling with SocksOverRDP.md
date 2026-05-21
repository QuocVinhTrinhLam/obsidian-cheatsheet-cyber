#### Loading SocksOverRDP.dll using regsvr32.exe

```cmd
C:\Users\htb-student\Desktop\SocksOverRDP-x64> regsvr32.exe SocksOverRDP-Plugin.dll
```
![[Pasted image 20260521133600.png]]
![[Pasted image 20260521133802.png]]
![[Pasted image 20260521133805.png]]
#### Confirming the SOCKS Listener is Started

```cmd
C:\Users\htb-student\Desktop\SocksOverRDP-x64> netstat -antb | findstr 1080
```
#### Configuring Proxifier
![[Pasted image 20260521133833.png]]
![[Pasted image 20260521133927.png]]
#### RDP Performance Considerations
![[Pasted image 20260521134030.png]]
