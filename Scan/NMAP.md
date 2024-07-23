https://nmap.org/book/host-discovery-strategies.html
```
nmap <scan types> <options> <target>
```
# Host Discovery
###### **TCP-SYN scan**
```
sudo nmap -sS localhost
```
###### **Scan Network Range**
```
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5
```

| Scanning Options | Description                                                      |
| ---------------- | ---------------------------------------------------------------- |
| -sn              | Disables port scanning.                                          |
| -oA tnet         | Stores the results in all formats starting with the name 'tnet'. |
| 10.129.2.0/24    | Target network range.                                            |
###### **Scan IP List**
```
sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5
```

| Scanning Options | Description                                                          |
| ---------------- | -------------------------------------------------------------------- |
| -oA tnet         | Stores the results in all formats starting with the name             |
| -iL              | Performs defined scans against targets in provided 'hosts.lst' list. |
| -sn              | Disables port scanning.                                              |
###### **Scan Multiple IPs**
```
sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20| grep for | cut -d" " -f5
```
or
```
sudo nmap -sn -oA tnet 10.129.2.18-20| grep for | cut -d" " -f5
```
###### **Scan Single IP**
```
sudo nmap 10.129.2.18 -sn -oA host 
```

| Scanning Options | Description                                                      |
| ---------------- | ---------------------------------------------------------------- |
| 10.129.2.18      | Performs defined scans against the target.                       |
| -sn              | Disables port scanning.                                          |
| -oA host         | Stores the results in all formats starting with the name 'host'. |
```
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace 
```
| Scanning Options | Description                                                              |
| ---------------- | ------------------------------------------------------------------------ |
| 10.129.2.18      | Performs defined scans against the target.                               |
| -sn              | Disables port scanning.                                                  |
| -oA host         | Stores the results in all formats starting with the name 'host'.         |
| -PE              | Performs the ping scan by using 'ICMP Echo requests' against the target. |
| --packet-trace   | Shows all packets sent and received                                      |
```
sudo nmap 10.129.2.18 -sn -oA host -PE --reason 
```

| Scanning Options | Description                                                              |
| ---------------- | ------------------------------------------------------------------------ |
| 10.129.2.18      | Performs defined scans against the target.                               |
| -sn              | Disables port scanning.                                                  |
| -oA host         | Stores the results in all formats starting with the name 'host'.         |
| -PE              | Performs the ping scan by using 'ICMP Echo requests' against the target. |
| --reason         | Displays the reason for specific result.                                 |
###### **Disable ARP pings**
```
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping 
```
###### https://nmap.org/book/host-discovery-strategies.html
# Host and Port Scanning
## Discovering Open TCP Ports
#### Scanning Top 10 TCP Ports
```nmap
sudo nmap 10.129.2.28 --top-ports=10 
```
#### Nmap - Trace the Packets
```nmap
sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping
```

`--packet-trace`|Shows all packets sent and received.
`-n`|Disables DNS resolution.
`--disable-arp-ping`|Disables ARP ping.
## Discovering Open UDP Ports
#### UDP Port Scan
```nmap
sudo nmap 10.129.2.28 -F -sU
```
#### Version Scan
```nmap
sudo nmap 10.129.2.28 -Pn -n --disable-arp-ping --packet-trace -p 445 --reason  -sV
```
# Saving the Results
## Different Formats
- Normal output (`-oN`) with the `.nmap` file extension
- Grepable output (`-oG`) with the `.gnmap` file extension
- XML output (`-oX`) with the `.xml` file extension

(`-oA`) to save the results in all formats
```nmap
sudo nmap 10.129.2.28 -p- -oA target
```
## Style sheets (XML)
```xsltproc
xsltproc target.xml -o target.html
```
![[nmap-results.png]]
# Service Enumeration
## Service Version Detection
```nmap
sudo nmap 10.129.2.28 -p- -sV
```

```nmap
sudo nmap 10.129.2.28 -p- -sV --stats-every=5s
```
|`--stats-every=5s`|Shows the progress of the scan every 5 seconds.|
###### Verbose
```nmap
sudo nmap 10.129.2.28 -p- -sV -v 
```
## Banner Grabbing
```nmap
sudo nmap 10.129.2.28 -p- -sV -Pn -n --disable-arp-ping --packet-trace
```
|`-Pn`|Disables ICMP Echo requests.|
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|
|`--packet-trace`|Shows all packets sent and received.|
## Tcpdump
```tcpdump
sudo tcpdump -i eth0 host 10.10.14.2 and 10.129.2.28
```
## Nc
```nc
nc -nv 10.129.2.28 25
```
# Nmap Scripting Engine

| **Category**  | **Description**                                                                                                     |
| ------------- | ------------------------------------------------------------------------------------------------------------------- |
| **auth**      | Determination of authentication credentials.                                                                        |
| **broadcast** | Scripts used for host discovery by broadcasting; discovered hosts can be automatically added to remaining scans.    |
| **brute**     | Executes scripts that try to log in to the respective service by brute-forcing with credentials.                    |
| **default**   | Default scripts executed by using the `-sC` option.                                                                 |
| **discovery** | Evaluation of accessible services.                                                                                  |
| **dos**       | Scripts used to check services for denial of service vulnerabilities, used less as it harms the services.           |
| **exploit**   | Scripts that try to exploit known vulnerabilities for the scanned port.                                             |
| **external**  | Scripts that use external services for further processing.                                                          |
| **fuzzer**    | Scripts that identify vulnerabilities and unexpected packet handling by sending different fields, taking much time. |
| **intrusive** | Intrusive scripts that could negatively affect the target system.                                                   |
| **malware**   | Checks if malware infects the target system.                                                                        |
| **safe**      | Defensive scripts that do not perform intrusive and destructive access.                                             |
| **version**   | Extension for service detection.                                                                                    |
| **vuln**      | Identification of specific vulnerabilities.                                                                         |
#### Default Scripts
```nmap
sudo nmap <target> -sC
```
#### Specific Scripts Category
```nmap
sudo nmap <target> --script <category>
```
#### Defined Scripts
```nmap
sudo nmap <target> --script <script-name>,<script-name>,...
```
#### Nmap - Specifying Scripts
```nmap
sudo nmap 10.129.2.28 -p 25 --script banner,smtp-commands
```
#### Nmap - Aggressive Scan
```nmap
sudo nmap 10.129.2.28 -p 80 -A
```
#### Nmap - Vuln Category
```nmap
sudo nmap 10.129.2.28 -p 80 -sV --script vuln 
```
# Performance
## Timeouts
#### Default Scan
```nmap
sudo nmap 10.129.2.0/24 -F
```
#### Optimized RTT
```nmap
sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms
```
|`-F`|Scans top 100 ports.|
|`--initial-rtt-timeout 50ms`|Sets the specified time value as initial RTT timeout.|
|`--max-rtt-timeout 100ms`|Sets the specified time value as maximum RTT timeout.|
## Max Retries
#### Default Scan
```nmap
sudo nmap 10.129.2.0/24 -F | grep "/tcp" | wc -l
```
#### Reduced Retries
```nmap
sudo nmap 10.129.2.0/24 -F --max-retries 0 | grep "/tcp" | wc -l
```
|`--max-retries 0`|Sets the number of retries that will be performed during the scan.|
## Rates
#### Default Scan
```nmap
sudo nmap 10.129.2.0/24 -F -oN tnet.default
```
#### Optimized Scan
```nmap
sudo nmap 10.129.2.0/24 -F -oN tnet.minrate300 --min-rate 300
```
|`-oN tnet.minrate300`|Saves the results in normal formats, starting the specified file name.|
|`--min-rate 300`|Sets the minimum number of packets to be sent per second.|
#### Default Scan - Found Open Ports
```nmap
cat tnet.default | grep "/tcp" | wc -l
```
#### Optimized Scan - Found Open Ports
```nmap
cat tnet.minrate300 | grep "/tcp" | wc -l
```
## Timing
These values (`0-5`) determine the aggressiveness of our scans.
- `-T 0` / `-T paranoid`
- `-T 1` / `-T sneaky`
- `-T 2` / `-T polite`
- `-T 3` / `-T normal`
- `-T 4` / `-T aggressive`
- `-T 5` / `-T insane`
#### Default Scan
```nmap
sudo nmap 10.129.2.0/24 -F -oN tnet.default 
```
#### Insane Scan
```nmap
sudo nmap 10.129.2.0/24 -F -oN tnet.T5 -T 5
```
|`-oN tnet.T5`|Saves the results in normal formats, starting the specified file name.|
|`-T 5`|Specifies the insane timing template.|
#### Default Scan - Found Open Ports
```nmap
cat tnet.default | grep "/tcp" | wc -l
```
#### Insane Scan - Found Open Ports
```nmap
cat tnet.T5 | grep "/tcp" | wc -l
```
# Firewall and IDS/IPS Evasion
#### SYN-Scan
```nmap
sudo nmap 10.129.2.28 -p 21,22,25 -sS -Pn -n --disable-arp-ping --packet-trace
```
#### ACK-Scan
```nmap
sudo nmap 10.129.2.28 -p 21,22,25 -sA -Pn -n --disable-arp-ping --packet-trace
```
|`-sS`|Performs SYN scan on specified ports.|
|`-sA`|Performs ACK scan on specified ports.|
|`-Pn`|Disables ICMP Echo requests.|
|`-n`|Disables DNS resolution.|
|`--disable-arp-ping`|Disables ARP ping.|
|`--packet-trace`|Shows all packets sent and received.|
## Decoys
#### Scan by Using Decoys
```nmap
sudo nmap 10.129.2.28 -p 80 -sS -Pn -n --disable-arp-ping --packet-trace -D RND:5
```
|`-D RND:5`|Generates five random IP addresses that indicates the source IP the connection comes from.|
#### Testing Firewall Rule
```nmap
sudo nmap 10.129.2.28 -n -Pn -p445 -O
```
#### Scan by Using Different Source IP
```nmap
sudo nmap 10.129.2.28 -n -Pn -p 445 -O -S 10.129.2.200 -e tun0
```
|`-O`|Performs operation system detection scan.|
|`-S`|Scans the target by using different source IP address.|
|`-e tun0`|Sends all requests through the specified interface.|
## DNS Proxying
#### SYN-Scan of a Filtered Port
```nmap
sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace
```
#### SYN-Scan From DNS Port
```nmap
sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace --source-port 53
```
#### Connect To The Filtered Port
```ncat
ncat -nv --source-port 53 10.129.2.28 50000
```
IDS/IPS
```shell
sudo nmap  -Pn --disable-arp-ping -p53 -sU -sC <TARGET>
```