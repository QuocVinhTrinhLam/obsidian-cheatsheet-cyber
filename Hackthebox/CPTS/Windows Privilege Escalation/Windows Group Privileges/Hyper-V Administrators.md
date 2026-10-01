#### Target File

```shell
C:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe
```
#### Taking Ownership of the File

```cmd
C:\htb> takeown /F C:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe
```
#### Starting the Mozilla Maintenance Service

```cmd
C:\htb> sc.exe start MozillaMaintenance
```
