https://nmap.org/book/host-discovery-strategies.html
## Basic Commands
```shell
nmap <TARGET>
```

```shell
nmap -sV -sC -p- <TARGET>
```
## SYN
```shell
sudo nmap -sS <TARGET>
```
## Scan Network Range
```shell
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5
```
|`10.129.2.0/24`|Target network range.|
|`-sn`|Disables port scanning.|
|`-oA tnet`|Stores the results in all formats starting with the name 'tnet'.|
## Scan IP List
```shell
sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5
```
|`-sn`|Disables port scanning.|
|`-oA tnet`|Stores the results in all formats starting with the name 'tnet'.|
|`-iL`|Performs defined scans against targets in provided 'hosts.lst' list.|
## Scan Multiple IPs
```shell
sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20| grep for | cut -d" " -f5
```
or
```shell
sudo nmap -sn -oA tnet 10.129.2.18-20| grep for | cut -d" " -f5
```
## Scan Single IP
```shell
sudo nmap 10.129.2.18 -sn -oA host 
```

```shell
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --reason 
```
|`-PE`|Performs the ping scan by using 'ICMP Echo requests' against the target.|
|`--packet-trace`|Shows all packets sent and received|
|`--reason`|Displays the reason for specific result.|

```shell
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping 
```
## Scanning Top 10 TCP Ports
```shell
sudo nmap 10.129.2.28 --top-ports=10 
```
## Nmap - Trace the Packets
```shell
sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping
```
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|
## Connect Scan
```shell
sudo nmap 10.129.2.28 -p 443 --packet-trace --disable-arp-ping -Pn -n --reason -sT 
```
## Filtered Ports

## Nmap Scripts
```shell
locate scripts/citrix
```
## FTP
```shell
nmap -sC -sV -p21 <IP>
```
## SMB
```shell
nmap --script smb-os-discovery.nse -p445 <IP>
```

```shell
nmap -A -p445 <IP>
```

