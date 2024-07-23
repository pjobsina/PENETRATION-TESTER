https://github.com/OJ/gobuster
#### Directory/File Enumeration
```gobuster
gobuster dir -u http://<IP>/ -w 
/usr/share/dirb/wordlists/common.txt
```
#### DNS Subdomain Enumeration
```gobuster
gobuster dns -d inlanefreight.com -w /home/kali/Desktop/SecLists/Discovery/DNS/namelist.txt
```