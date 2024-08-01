# Linux
```shell
ifconfig
```
**Each NIC has an identifier (`eth0`, `eth1`, `lo`, `tun0`)**
- The tunnel interface (tun0) indicates a VPN connection is active.
	- The VPN encrypts traffic and also establishes a tunnel over a public network (often the Internet), through `NAT` on a public-facing network appliance, and into the internal/private network.
- The IP assigned to eth0 (`134.122.100.200`) is a publicly routable IP address.
	- Meaning ISPs will route traffic originating from this IP over the Internet.
# Windows
```powershell
ipconfig
```
**There are [IPv6](https://www.cisco.com/c/en/us/solutions/ipv6/overview.html) addresses and an [IPv4](https://en.wikipedia.org/wiki/IPv4) address.**
- Every IPv4 address will have a corresponding `subnet mask`. If an IP address is like a phone number, the subnet mask is like the area code.
- Remember that the subnet mask defines the `network` & `host` portion of an IP address.