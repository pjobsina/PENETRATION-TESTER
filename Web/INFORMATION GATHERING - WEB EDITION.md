# Active Reconnaissance
`directly interacts with the target system` to gather information.

| **Technique**              | **Description**                                                                               | **Tools**                                                  |
| -------------------------- | --------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| **Port Scanning**          | Identifying open ports and services running on the target.                                    | Nmap, Masscan, Unicornscan                                 |
| **Vulnerability Scanning** | Probing the target for known vulnerabilities, such as outdated software or misconfigurations. | Nessus, OpenVAS, Nikto                                     |
| **Network Mapping**        | Mapping the target's network topology, including connected devices and their relationships.   | Traceroute, Nmap                                           |
| **Banner Grabbing**        | Retrieving information from banners displayed by services running on the target.              | Netcat, curl                                               |
| **OS Fingerprinting**      | Identifying the operating system running on the target.                                       | Nmap, Xprobe2                                              |
| **Service Enumeration**    | Determining the specific versions of services running on open ports.                          | Nmap                                                       |
| **Web Spidering**          | Crawling the target website to identify web pages, directories, and files.                    | Burp Suite Spider, OWASP ZAP Spider, Scrapy (customisable) |
# Passive Reconnaissance
gathering information about the target `without directly interacting` with it.

| **Technique**             | **Description**                                                                                                                 | **Tools**                                             |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| **Search Engine Queries** | Utilizing search engines to uncover information about the target, including websites, social media profiles, and news articles. | Google, DuckDuckGo, Bing, Shodan                      |
| **WHOIS Lookups**         | Querying WHOIS databases to retrieve domain registration details.                                                               | whois command-line tool, online WHOIS lookup services |
| **DNS**                   | Analyzing DNS records to identify subdomains, mail servers, and other infrastructure.                                           | dig, nslookup, host, dnsenum, fierce, dnsrecon        |
| **Web Archive Analysis**  | Examining historical snapshots of the target's website to identify changes, vulnerabilities, or hidden information.             | Wayback Machine                                       |
| **Social Media Analysis** | Gathering information from social media platforms like LinkedIn, Twitter, or Facebook.                                          | LinkedIn, Twitter, Facebook, specialized OSINT tools  |
| **Code Repositories**     | Analyzing publicly accessible code repositories like GitHub for exposed credentials or vulnerabilities.                         | GitHub, GitLab                                        |
# WHOIS
```whois
whois inlanefreight.com
```

# DIG (Domain Information Groper)
### Common dig Commands

| Command                         | Description                                                                                                   |
|---------------------------------|---------------------------------------------------------------------------------------------------------------|
| `dig domain.com`                | Performs a default A record lookup for the domain.                                                            |
| `dig domain.com A`              | Retrieves the IPv4 address (A record) associated with the domain.                                             |
| `dig domain.com AAAA`           | Retrieves the IPv6 address (AAAA record) associated with the domain.                                          |
| `dig domain.com MX`             | Finds the mail servers (MX records) responsible for the domain.                                               |
| `dig domain.com NS`             | Identifies the authoritative name servers for the domain.                                                     |
| `dig domain.com TXT`            | Retrieves any TXT records associated with the domain.                                                         |
| `dig domain.com CNAME`          | Retrieves the canonical name (CNAME) record for the domain.                                                   |
| `dig domain.com SOA`            | Retrieves the start of authority (SOA) record for the domain.                                                 |
| `dig @1.1.1.1 domain.com`       | Specifies a specific name server to query; in this case 1.1.1.1                                               |
| `dig +trace domain.com`         | Shows the full path of DNS resolution.                                                                        |
| `dig -x 192.168.1.1`            | Performs a reverse lookup on the IP address 192.168.1.1 to find the associated host name.                      |
| `dig +short domain.com`         | Provides a short, concise answer to the query.                                                                |
| `dig +noall +answer domain.com` | Displays only the answer section of the query output.                                                         |
| `dig domain.com ANY`            | Retrieves all available DNS records for the domain (Note: Many DNS servers ignore ANY queries to reduce load and prevent abuse, as per RFC 8482). |
# Subdomains
## Subdomain Bruteforcing

| Tool         | Description                                                                                                              |
|--------------|--------------------------------------------------------------------------------------------------------------------------|
| dnsenum      | Comprehensive DNS enumeration tool that supports dictionary and brute-force attacks for discovering subdomains.          |
| fierce       | User-friendly tool for recursive subdomain discovery, featuring wildcard detection and an easy-to-use interface.         |
| dnsrecon     | Versatile tool that combines multiple DNS reconnaissance techniques and offers customizable output formats.              |
| amass        | Actively maintained tool focused on subdomain discovery, known for its integration with other tools and extensive data sources. |
| assetfinder  | Simple yet effective tool for finding subdomains using various techniques, ideal for quick and lightweight scans.         |
| puredns      | Powerful and flexible DNS brute-forcing tool, capable of resolving and filtering results effectively.                      |
```dnsenum
dnsenum --enum inlanefreight.com -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -r
```
# DNS Zone Transfers
## The Zone Transfer Vulnerability
#### Exploiting Zone Transfers
```dig
dig axfr <DNS> @<TARGET IP>
```

```dig
dig axfr inlanefreight.htb @<TARGET IP> | grep "ftp.admin.inlanefreight.htb"
```
# Virtual Hosts
### Server VHost Lookup
![[vhost.png]]
### gobuster
```gobuster
gobuster vhost -u http://<target_IP_address> -w <wordlist_file> --append-domain
```

```gobuster
gobuster vhost -u http://inlanefreight.htb:81 -w /home/kali/Desktop/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
```
The `--append-domain` flag appends the base domain to each word in the wordlist.
# Certificate Transparency Logs
### crt.sh lookup
```curl
curl -s "https://crt.sh/?q=facebook.com&output=json" | jq -r '.[]
 | select(.name_value | contains("dev")) | .name_value' | sort -u
```
# Fingerprinting
### Banner Grabbing
```curl
curl -I inlanefreight.com
```

```curl
curl -I https://inlanefreight.com
```
### Wafw00f
```wafw00f
wafw00f inlanefreight.com
```
### Nikto
```nikto
nikto -h inlanefreight.com -Tuning b
```
The `-Tuning b` flag tells `Nikto` to only run the Software Identification modules
# Creepy Crawlies
## Popular Web Crawlers

| **Tool**                  | **Description**                                                                                                    |
| --------------------- | -------------------------------------------------------------------------------------------------------------- |
| **Burp Suite Spider** | Burp Suite, a widely used web application testing platform, includes a powerful active crawler called Spider.  |
| **OWASP ZAP Spider**  | ZAP is a free, open-source web application security scanner with a spider component to crawl web applications. |
| **Scrapy**            | A versatile and scalable Python framework for building custom web crawlers.                                    |
| **Apache Nutch**      | A highly extensible and scalable open-source web crawler written in Java.                                      |
### ReconSpider
```wget
wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip
unzip ReconSpider.zip 
```

```py
python3 ReconSpider.py http://inlanefreight.com
```
After running `ReconSpider.py`, the data will be saved in a JSON file, `results.json`.
# Automating Recon
## Reconnaissance Frameworks
These frameworks aim to provide a complete suite of tools for web reconnaissance:

| Tool                                                     | Description                                                                                                   |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| [FinalRecon](https://github.com/thewhiteh4t/FinalRecon)  | A Python-based reconnaissance tool offering a range of modules for different tasks.                           |
| [Recon-ng](https://github.com/lanmaster53/recon-ng)      | A powerful framework written in Python with various modules for different reconnaissance tasks.               |
| [theHarvester](https://github.com/laramies/theHarvester) | Designed for gathering email addresses, subdomains, hosts, employee names, open ports, and banners.           |
| [SpiderFoot](https://github.com/smicallef/spiderfoot)    | An open-source intelligence automation tool that integrates with various data sources to collect information. |
| [OSINT Framework](https://osintframework.com/)           | A collection of various tools and resources for open-source intelligence gathering.                           |
### FinalRecon
```git
git clone https://github.com/thewhiteh4t/FinalRecon.git
cd FinalRecon
pip3 install -r requirements.txt
chmod +x ./finalrecon.py
./finalrecon.py --help
```

