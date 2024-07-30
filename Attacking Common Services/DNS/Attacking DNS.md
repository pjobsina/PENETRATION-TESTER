- The [Domain Name System](https://www.cloudflare.com/learning/dns/what-is-dns/) (`DNS`) translates domain names (e.g., hackthebox.com) to the numerical IP addresses (e.g., 104.17.42.72). 
- DNS is mostly `UDP/53`, but DNS will rely on `TCP/53` more heavily as time progresses.
# Enumeration
```shell
nmap -p53 -Pn -sV -sC 10.10.110.213
```
# DNS Zone Transfer
#### DIG - AXFR Zone Transfer
```shell
dig AXFR @ns1.inlanefreight.htb inlanefreight.htb
```
#### Fierce
Tools like [Fierce](https://github.com/mschwager/fierce) can also be used to enumerate all DNS servers of the root domain and scan for a DNS zone transfer:
```shell
fierce --domain zonetransfer.me
```
# Domain Takeovers & Subdomain Enumeration
- `Domain takeover` is registering a non-existent domain name to gain control over another domain.
- Domain takeover is also possible with subdomains called `subdomain takeover`.
### Subdomain Enumeration
#### Subfinder
```shell
./subfinder -d inlanefreight.com -v       
```
#### Subbrute
```shell
git clone https://github.com/TheRook/subbrute.git >> /dev/null 2>&1
cd subbrute
```
```shell
echo <IP> > resolvers.txt
python3 subbrute.py inlanefreight.htb -s /usr/share/seclists/Discovery/DNS/namelist.txt -r resolvers.txt
```

Using the `nslookup` or `host` command, we can enumerate the `CNAME` records for those subdomains.
```shell
host support.inlanefreight.com
```
# DNS Spoofing
- DNS spoofing is also referred to as DNS Cache Poisoning. 
- This attack involves altering legitimate DNS records with false information so that they can be used to redirect online traffic to a fraudulent website. 
- Example attack paths for the DNS Cache Poisoning are as follows:
	- An attacker could intercept the communication between a user and a DNS server to route the user to a fraudulent destination instead of a legitimate one by performing a Man-in-the-Middle (`MITM`) attack.
	- Exploiting a vulnerability found in a DNS server could yield control over the server by an attacker to modify the DNS records.
#### Local DNS Cache Poisoning
```shell
cat /etc/ettercap/etter.dns
```
Start the `Ettercap` tool and scan for live hosts within the network by navigating to `Hosts > Scan for Hosts`. Once completed, add the target IP address (e.g., `192.168.152.129`) to Target1 and add a default gateway IP (e.g., `192.168.152.2`) to Target2.

Activate `dns_spoof` attack by navigating to `Plugins > Manage Plugins`. This sends the target machine with fake DNS responses that will resolve `inlanefreight.com` to IP address `192.168.225.110`.

After a successful DNS spoof attack, if a victim user coming from the target machine `192.168.152.129` visits the `inlanefreight.com` domain on a web browser, they will be redirected to a `Fake page` that is hosted on IP address `192.168.225.110`:
![](https://academy.hackthebox.com/storage/modules/116/etter_site.png)

```cmd
ping inlanefreight.com
```

```
dig AXFR @ns1.inlanefreight.htb inlanefreight.htb
```
