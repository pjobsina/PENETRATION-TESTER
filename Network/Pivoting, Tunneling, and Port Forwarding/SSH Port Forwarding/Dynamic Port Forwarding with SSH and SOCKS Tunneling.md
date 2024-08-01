# Port Forwarding
- `Port forwarding` is a technique that allows us to redirect a communication request from one port to another. 
- Port forwarding uses TCP as the primary communication layer to provide interactive communication for the forwarded port.
- However, different application layer protocols such as SSH or even [SOCKS](https://en.wikipedia.org/wiki/SOCKS) (non-application layer) can be used to encapsulate the forwarded traffic.
- This can be effective in bypassing firewalls and using existing services on your compromised host to pivot to other networks.
# SSH Local Port Forwarding
![[SSH-port-forwarding.png]]
### Scanning the Pivot Target
```shell
nmap -sT -p22,3306 10.129.202.64
```
### Executing the Local Port Forward
```shell
ssh -L 1234:localhost:3306 ubuntu@10.129.202.64
```
### Confirming Port Forward with Netstat
```shell
netstat -antp | grep 1234
```
### Confirming Port Forward with Nmap
```shell
nmap -v -sV -p1234 localhost
```
### Forwarding Multiple Ports
```shell
ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@10.129.202.64
```
## Setting up to Pivot
Now, if you type `ifconfig` on the Ubuntu host, you will find that this server has multiple NICs:
- One connected to our attack host (`ens192`)
- One communicating to other hosts within a different network (`ens224`)
- The loopback interface (`lo`).
### Looking for Opportunities to Pivot using ifconfig
```shell
ifconfig
```
This is called `SSH tunneling` over `SOCKS proxy`. SOCKS stands for `Socket Secure`, a protocol that helps communicate with servers where you have firewall restrictions in place
![[socks.png]]
#### Enabling Dynamic Port Forwarding with SSH
```shell
ssh -D 9050 ubuntu@10.129.202.64
```
#### Checking /etc/proxychains.conf
```shell
tail -4 /etc/proxychains.conf
```
#### Using Nmap with Proxychains
```shell
proxychains nmap -v -sn 172.16.5.1-200
```
#### Enumerating the Windows Target through Proxychains
```shell
proxychains nmap -v -Pn -sT 172.16.5.19
```
## Using Metasploit with Proxychains
```shell
proxychains msfconsole
```
#### Using rdp_scanner Module
```msfconsole
search rdp_scanner
use 0
set rhosts 172.16.5.19
run
```
#### Using xfreerdp with Proxychains
```shell
proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```
