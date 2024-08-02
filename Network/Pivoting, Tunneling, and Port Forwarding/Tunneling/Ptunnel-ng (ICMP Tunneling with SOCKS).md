- ICMP tunneling encapsulates your traffic within `ICMP packets` containing `echo requests` and `responses`. 
- ICMP tunneling would only work when ping responses are permitted within a firewalled network. 
- When a host within a firewalled network is allowed to ping an external server, it can encapsulate its traffic within the ping echo request and send it to an external server. 
## ptunnel-ng
```shell
git clone https://github.com/utoni/ptunnel-ng.git
cd ptunnel-ng
sudo ./autogen.sh 
```
#### Transferring Ptunnel-ng to the Pivot Host
```shell
scp -r ptunnel-ng ubuntu@10.129.202.64:~/
```
#### Starting the ptunnel-ng Server on the Target Host
```shell
./ptunnel-ng -r10.129.202.64 -R22
```
#### Connecting to ptunnel-ng Server from Attack Host
```shell
sudo ./ptunnel-ng -p10.129.202.64 -l2222 -r10.129.202.64 -R22
```
With the ptunnel-ng ICMP tunnel successfully established, we can attempt to connect to the target using SSH through local port 2222 (`-p2222`).
#### Tunneling an SSH connection through an ICMP Tunnel
```shell
ssh -p2222 -lubuntu 127.0.0.1
```
#### Viewing Tunnel Traffic Statistics
```shell
inf]: Incoming tunnel request from 10.10.14.18.
[inf]: Starting new session to 10.129.202.64:22 with ID 20199
[inf]: Received session close from remote peer.
[inf]: 
Session statistics:
[inf]: I/O:   0.00/  0.00 mb ICMP I/O/R:      248/      22/       0 Loss:  0.0%
```
#### Enabling Dynamic Port Forwarding over SSH
```shell
ssh -D 9050 -p2222 -lubuntu 127.0.0.1
```
#### Proxychaining through the ICMP Tunnel
```shell
proxychains nmap -sV -sT 172.16.5.19 -p3389
```
## Network Traffic Analysis Considerations
![[analyzingTheTraffic.gif]]
a connection is established over SSH without using ICMP tunneling. We may notice that `TCP` & `SSHv2` traffic is captured.

