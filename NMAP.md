#### Basic Commands
```shell
nmap <IP>
```

```shell
nmap -sV -sC -p- <IP>
```
#### Nmap Scripts
```shell
locate scripts/citrix
```
#### FTP
```shell
nmap -sC -sV -p21 <IP>
```
#### SMB
```shell
nmap --script smb-os-discovery.nse -p445 <IP>
```

```shell
nmap -A -p445 <IP>
```

