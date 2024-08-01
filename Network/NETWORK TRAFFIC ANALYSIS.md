![[OSI-TCPIP-PDU.png]]
###### PDU PACKETS
![[pdu-packet.png]]
#### Media Access Control (`MAC`) Address
- 48-bit `six octet` address represented in hexadecimal format.
- It is utilized in Layer two ( `the data-link or link-layer depending on which model` ) communications between hosts.
#### Internet Protocol (`IP`) Address
- was developed to deliver data from one host to another across network boundaries.
## TCP vs. UDP

| Characteristic            | TCP                                                                         | UDP                                                      |
|---------------------------|-----------------------------------------------------------------------------|----------------------------------------------------------|
| Transmission              | Connection-oriented                                                         | Connectionless. Fire and forget.                         |
| Connection Establishment  | TCP uses a three-way handshake to ensure that a connection is established.  | UDP does not ensure the destination is listening.        |
| Data Delivery             | Stream-based conversations                                                  | Packet by packet, the source does not care if the destination is active |
| Receipt of data           | Sequence and Acknowledgement numbers are utilized to account for data.      | UDP does not care.                                       |
| Speed                     | TCP has more overhead and is slower because of its built-in functions.       | UDP is fast but unreliable.                              |
## TCP Threeway Handshake
![[threeway-handshake.png]]
- **Step 1 (SYN):** In the first step, the client wants to establish a connection with a server, so it sends a segment with SYN(Synchronize Sequence Number) which informs the server that the client is likely to start communication and with what sequence number it starts segments with.
- **Step 2 (SYN + ACK):** Server responds to the client request with SYN-ACK signal bits set. Acknowledgement(ACK) signifies the response of the segment it received and SYN signifies with what sequence number it is likely to start the segments with.
- **Step 3 (ACK):** In the final part client acknowledges the response of the server and they both establish a reliable connection with which they will start the actual data transfer.
## HTTP (Hypertext Transfer Protocol)
- It enables the transfer of data in clear text between a client and server over TCP.
#### HTTP Methods
- HEAD
- GET
- POST
- PUT
- DELETE
- TRACE
- OPTIONS
- CONNECT
## HTTPS (Hypertext Transfer Protocol Secure)
- It is a modification of the HTTP protocol designed to utilize Transport Layer Security (`TLS`) or Secure Sockets Layer (`SSL`) with older applications for data security.
- TLS is utilized as an encryption mechanism to secure the communications between a client and a server.
- HTTPS utilizes ports **443** and **8443** instead of the standard port 80.
# Tcpdump
- It is a command-line packet sniffer that can directly capture and interpret data frames from a file or network interface. 
- It was built for use on any Unix-like operating system and had a Windows twin called `WinDump`.

| Switch Command | Result                                                                                                             |
| :------------: | ------------------------------------------------------------------------------------------------------------------ |
|       D        | Will display any interfaces available to capture from.                                                             |
|       i        | Selects an interface to capture from. ex. -i eth0                                                                  |
|       n        | Do not resolve hostnames.                                                                                          |
|       nn       | Do not resolve hostnames or well-known ports.                                                                      |
|       e        | Will grab the ethernet header along with upper-layer data.                                                         |
|       X        | Show contents of packets in hex and ASCII.                                                                         |
|       XX       | Same as X, but will also specify ethernet headers (like using Xe).                                                 |
|   v, vv, vvv   | Increase the verbosity of output shown and saved.                                                                  |
|       c        | Grab a specific number of packets, then quit the program.                                                          |
|       s        | Defines how much of a packet to grab.                                                                              |
|       S        | Change relative sequence numbers in the capture display to absolute sequence numbers. (13248765839 instead of 101) |
|       q        | Print less protocol information.                                                                                   |
|  r file.pcap   | Read from a file.                                                                                                  |
|  w file.pcap   | Write into a file                                                                                                  |
#### Locate Tcpdump
```shell
which tcpdump
```
#### Install Tcpdump
```shell
sudo apt install tcpdump 
```
#### Tcpdump Version Validation
```shell
sudo tcpdump --version
```
#### Listing Available Interfaces
```shell
sudo tcpdump -D
```
#### Choosing an Interface to Capture From
```shell
sudo tcpdump -i eth0
```
#### Disable Name Resolution
```shell
sudo tcpdump -i eth0 -nn
```
#### Display the Ethernet Header
```shell
sudo tcpdump -i eth0 -e
```
#### Include ASCII and Hex Output
```shell
sudo tcpdump -i eth0 -X
```
#### Tcpdump Switch Combinations
```shell
sudo tcpdump -i eth0 -nnvXX
```
## Tcpdump Output
![[tcp-dump-output.png]]
## File Input/Output with Tcpdump
#### Save our PCAP Output to a File
```shell
sudo tcpdump -i eth0 -w ~/output.pcap
```
#### Reading Output From a File
```shell
sudo tcpdump -r ~/output.pcap
```
# Tcpdump Packet Filtering

|        Filter        | Result                                                                                     |
| :------------------: | ------------------------------------------------------------------------------------------ |
|         host         | Filters visible traffic to show anything involving the designated host. Bi-directional.    |
|      src / dest      | Modifiers to designate a source or destination host or port.                               |
|         net          | Shows any traffic sourcing from or destined to the designated network. It uses / notation. |
|        proto         | Filters for a specific protocol type. (e.g., ether, TCP, UDP, ICMP)                        |
|         port         | Bi-directional. Shows any traffic with the specified port as the source or destination.    |
|      portrange       | Allows specifying a range of ports. (e.g., 0-1024)                                         |
| less / greater "< >" | Used to look for a packet or protocol option of a specific size.                           |
|       and / &&       | Used to concatenate two different filters together. (e.g., src host AND port)              |
|          or          | Allows for a match on either of two conditions. Does not have to meet both. Can be tricky. |
|         not          | Modifier saying anything but x. (e.g., not UDP)                                            |
#### Host Filter
```shell
sudo tcpdump -i eth0 host 172.16.146.2
```
#### Source/Destination Filter
```shell
sudo tcpdump -i eth0 src host 172.16.146.2
```
#### Utilizing Source With Port as a Filter
```shell
sudo tcpdump -i eth0 tcp src port 80
```
#### Using Destination in Combination with the Net Filter
```shell
sudo tcpdump -i eth0 dest net 172.16.146.0/24
```
#### Protocol Filter
```shell
sudo tcpdump -i eth0 udp
```
#### Protocol Number Filter
```shell
sudo tcpdump -i eth0 proto 17
```
#### Port Filter
```shell
sudo tcpdump -i eth0 tcp port 443
```
#### Port Range Filter
```shell
sudo tcpdump -i eth0 portrange 0-1024
```
#### Less/Greater Filter
```shell
sudo tcpdump -i eth0 less 64
```
#### Utilizing Greater
```shell
sudo tcpdump -i eth0 greater 500
```
#### AND Filter
```shell
sudo tcpdump -i eth0 host 192.168.0.1 and port 23
```
#### Basic Capture With No Filter
```shell
sudo tcpdump -i eth0
```
#### OR Filter
```shell
sudo tcpdump -r sus.pcap icmp or host 172.16.146.1
```
#### NOT Filter
```shell
sudo tcpdump -r sus.pcap not icmp
```
## Tips and Tricks
```shell
sudo tcpdump -Ar telnet.pcap
```
`-A` This can be helpful when quickly looking for something human-readable in the output.
#### Piping a Capture to Grep
```shell
sudo tcpdump -Ar http.cap -l | grep 'mailto:*'
```
#### Looking for TCP Protocol Flags
```shell
tcpdump -i eth0 'tcp[13] &2 != 0'
```
#### Hunting For a SYN Flag
```shell
sudo tcpdump -i eth0 'tcp[13] &2 != 0'
```
## Protocol RFC Links

|                                    Link                                    | Description                                                                                                   |
| :------------------------------------------------------------------------: | ------------------------------------------------------------------------------------------------------------- |
|             [IP Protocol](https://tools.ietf.org/html/rfc791)              | RFC 791 describes IP and its functionality.                                                                   |
|            [ICMP Protocol](https://tools.ietf.org/html/rfc792)             | RFC 792 describes ICMP and its functionality.                                                                 |
|             [TCP Protocol](https://tools.ietf.org/html/rfc793)             | RFC 793 describes the TCP protocol and how it functions.                                                      |
|             [UDP Protocol](https://tools.ietf.org/html/rfc768)             | RFC 768 describes UDP and how it operates.                                                                    |
| [RFC Quick Links](https://en.wikipedia.org/wiki/List_of_RFCs#Topical_list) | This Wikipedia article contains a large list of protocols tied to the RFC that explains their implementation. |
# WIRESHARK
- It is a free and open-source network traffic analyzer much like tcpdump but with a graphical interface.
![[wireshark.png]]
### Packet List: `Orange`
A summary line of each packet that includes the fields listed below by default. We can add or remove columns to change what information is presented.
- Number- Order the packet arrived in Wireshark
- Time - Unix time format
- Source - Source IP
- Destination - Destination IP
- Protocol - The protocol used (TCP, UDP, DNS, ETC.)
- Information - Information about the packet. This field can vary based on the type of protocol used within. It will show, for example, what type of query It is for a DNS packet.
### Packet Details: `Blue`
- The Packet Details window allows us to drill down into the packet to inspect the protocols with greater detail. It will break it down into chunks that we would expect following the typical OSI Model reference. `The packet is dissected into different encapsulation layers for inspection.`
- Keep in mind, Wireshark will show this encapsulation in reverse order with lower layer encapsulation at the top of the window and higher levels at the bottom.
### Packet Bytes: `Green`
- he Packet Bytes window allows us to look at the packet contents in ASCII or hex output. As we select a field from the windows above, it will be highlighted in the Packet Bytes window and show us where that bit or byte falls within the overall packet.
- This is a great way to validate that what we see in the Details pane is accurate and the interpretation Wireshark made matches the packet output.
- Each line in the output contains the data offset, sixteen hexadecimal bytes, and sixteen ASCII bytes. Non-printable bytes are replaced with a period in the ASCII format.
## Following TCP Streams
- right-click on a packet from the stream we wish to recreate.
- select follow → TCP
- this will open a new window with the stream stitched back together. From here, we can see the entire conversation.
## Filter For A Specific TCP Stream
```
tcp.stream eq 0
```
First three packet are full `TCP handshake`.
#### Extracting Data and Files From a Capture
- stop your capture.
- Select the File radial → Export → , then select the protocol format to extract from.
- (DICOM, HTTP, SMB, etc.)
## FTP Disector
- `ftp.request.command` - Will show any commands sent across the ftp-control channel ( port 21 )
    - We can look for information like usernames and passwords with this filter. It can also show us filenames for anything requested.
- `ftp-data` - Will show any data transferred over the data channel ( port 20 )
    - If we filter on a conversation and utilize `ftp-data`, we can capture anything sent during the conversation. We can reconstruct anything transferred by placing the raw data back into a new file and naming it appropriately.
