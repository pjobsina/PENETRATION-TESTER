# Hunting for Encoded Files
a useful list can be found on [FileInfo](https://fileinfo.com/filetypes/encoded)
#### Hunting for Files
```shell
for ext in $(echo ".xls .xls* .xltx .csv .od* .doc .doc* .pdf .pot .pot* .pp*");do echo -e "\nFile extension: " $ext; find / -name *$ext 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```
#### Hunting for SSH Keys
```shell
grep -rnw "PRIVATE KEY" /* 2>/dev/null | grep ":1"
```
#### Encrypted SSH Keys
```shell
cat /home/cry0l1t3/.ssh/SSH.private
```
# Cracking with John
#### John Hashing Scripts
```shell
locate *2john*
```
```shell
ssh2john.py SSH.private > ssh.hash
cat ssh.hash 
```
#### Cracking SSH Keys
```shell
john --wordlist=rockyou.txt ssh.hash
```
```shell
john ssh.hash --show
```
## Cracking Documents
#### Cracking Microsoft Office Documents
```shell
office2john.py Protected.docx > protected-docx.hash
cat protected-docx.hash
```
```shell
john --wordlist=rockyou.txt protected-docx.hash
```
```shell
john protected-docx.hash --show
```
#### Cracking PDFs
```shell
pdf2john.py PDF.pdf > pdf.hash
cat pdf.hash
```
```shell
john --wordlist=rockyou.txt pdf.hash
```
```shell
john pdf.hash --show
```


