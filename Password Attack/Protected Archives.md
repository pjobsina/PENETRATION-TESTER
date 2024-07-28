#### Download All File Extensions
```shell
curl -s https://fileinfo.com/filetypes/compressed | html2text | awk '{print tolower($1)}' | grep "\." | tee -a compressed_ext.txt
```
# Cracking Archives
### Cracking ZIP
#### Using zip2john
```shell
zip2john ZIP.zip > zip.hash
```
#### Viewing the Contents of zip.hash
```shell
cat zip.hash 
```
#### Cracking the Hash with John
```shell
john --wordlist=rockyou.txt zip.hash
```
#### Viewing the Cracked Hash
```shell
john zip.hash --show
```
## Cracking OpenSSL Encrypted Archives
#### Using a for-loop to Display Extracted Contents
```shell
for i in $(cat rockyou.txt);do openssl enc -aes-256-cbc -d -in GZIP.gzip -k $i 2>/dev/null| tar xz;done
```
## Cracking BitLocker Encrypted Drives
#### Using bitlocker2john
```shell
bitlocker2john -i Backup.vhd > backup.hashes
grep "bitlocker\$0" backup.hashes > backup.hash
cat backup.hash
```
#### Using hashcat to Crack backup.hash
```shell
hashcat -m 22100 backup.hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt -o backup.cracked
```
#### Viewing the Cracked Hash
```shell
cat backup.cracked 
```
