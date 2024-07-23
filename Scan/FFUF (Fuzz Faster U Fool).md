```
ffuf -h
```
# **DIRECTORY FUZZING**
```
ffuf -w <WORDLIST>:FUZZ -u http://SERVER_IP:PORT/FUZZ
```
# **EXTENSION FUZZING**
```
ffuf -w /home/kali/Desktop/SecLists/Discovery/Web-Content/web-extensions.txt:FUZZ -u <IP>/indexFUZZ
```
# **PAGE FUZZING**
```
ffuf -w /home/kali/Desktop/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/blog/FUZZ.php
```
# **RECURSIVE FUZZING**
```
ffuf -w /home/kali/Desktop/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v
```
# **SUB-DOMAIN FUZZING**
```
ffuf -w /home/kali/Desktop/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u https://FUZZ.inlanefreight.com/
```
# **VHOST FUZZING**
```
ffuf -w /home/kali/Desktop/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb'
```
# **FFUF FILTERS**

| Option | Description |
|--------|-------------|
| **MATCHER OPTIONS** | |
| -mc | Match HTTP status codes, or "all" for everything. (default: 200,204,301,302,307,401,403) |
| -ml | Match amount of lines in response |
| -mr | Match regexp |
| -ms | Match HTTP response size |
| -mw | Match amount of words in response |
| **FILTER OPTIONS** | |
| -fc | Filter HTTP status codes from response. Comma separated list of codes and ranges |
| -fl | Filter by amount of lines in response. Comma separated list of line counts and ranges |
| -fr | Filter regexp |
| -fs | Filter HTTP response size. Comma separated list of sizes and ranges |
| -fw | Filter by amount of words in response. Comma separated list of word counts and ranges |
# **GET Request Fuzzing**
```
ffuf -w /home/kali/Desktop/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=key -fs xxx
```
# **POST Request Fuzzing**
```
ffuf -w /home/kali/Desktop/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```
```
curl http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=key' -H 'Content-Type: application/x-www-form-urlencoded'
```
# **Value Fuzzing**
**Numbers 1-1000**
```
for i in $(seq 1 1000); do echo $i >> ids.txt; done
```
**Fuzzing**
```
ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```
