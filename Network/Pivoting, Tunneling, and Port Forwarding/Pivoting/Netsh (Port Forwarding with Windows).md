- [Netsh](https://docs.microsoft.com/en-us/windows-server/networking/technologies/netsh/netsh-contexts) is a Windows command-line tool that can help with the network configuration of a particular Windows system. 
- Here are just some of the networking related tasks we can use `Netsh` for:
	- `Finding routes`
	- `Viewing the firewall configuration`
	- `Adding proxies`
	- `Creating port forwarding rules`
![[Pasted image 20240801075253.png]]
#### Using Netsh.exe to Port Forward
```cmd
netsh.exe interface portproxy add v4tov4 listenport=8080 listenaddress=<target-ip> connectport=3389 connectaddress=<ip-server>
```
#### Verifying Port Forward
```cmd
netsh.exe interface portproxy show v4tov4
```
#### Connecting to the Internal Host through the Port Forward
```
xfreerdp /v:10.129.42.198:8080 /u:victor /p:pass@123
```
