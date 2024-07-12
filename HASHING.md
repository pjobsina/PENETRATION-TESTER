## md5sum
```shell
echo -n "p@ssw0rd" | md5sum
```
## Encryption
### Symmetric Encryption
```shell
python3
```
```python
from pwn import xor
xor("p@ssw0rd", "academy")
```
```python
from pwn import xor
xor('\x03%\x10\x01\x12D\x01\x01', "secret")
```
# Hashid
```shell
pip install hashid
```

```shell
hashid '$apr1$71850310$gh9m4xcAn3MGxogwX/ztb.'
```

```shell
hashid '$DCC2$10240#tom#e4e938d12fe5974dc42a90120bd9c90f' -m
```

```shell
hashid 'a2d1f7b7a1862d0d4a52644e72d59df5:500:lp@trash-mail.com'
```
# Hashcat
```shell
sudo apt install hashcat
```
```hashcat
hashcat -h
```
### Example Hashes
```shell
hashcat --example-hashes | less
```
### Benchmark
```shell
hashcat -b -m 0
```
### Dictionary Attack
```shell
hashcat -a 0 -m <hash type> <hash file> <wordlist>
```
#### Example
```shell
echo -n '!academy' | sha256sum | cut -f1 -d' ' > sha256_hash_example
```
```shell
hashcat -a 0 -m 0 sha256_hash_example /home/kali/Desktop/SecLists/Passwords/Leaked-Databases/rockyou.txt
```
#### Status
```shell
[s]tatus [p]ause [b]ypass [c]heckpoint [q]uit => s
```
### Combination Attack
```shell
cat wordlist1
```
`super`
`world`
`secret`
```shell
cat wordlist2
```
`hello`
`password`
```shell
awk '(NR==FNR) { a[NR]=$0 } (NR != FNR) { for (i in a) { print $0 a[i] } }' wordlist2 wordlist1
```

```shell
hashcat -a 1 --stdout wordlist1 wordlist2
```
###### Syntax
```shell
hashcat -a 1 -m <hash type> <hash file> <wordlist1> <wordlist2>
```

```shell
echo -n 'secretpassword' | md5sum | cut -f1 -d' '  > combination_md5
```

```shell
hashcat -a 1 -m 0 combination_md5 wordlist1 wordlist2
```
### Mask Attack
#### Creating MD5 hashes
```shell
echo -n 'ILFREIGHTabcxy2015' | md5sum | tr -d " -" > md5_mask_example_hash
```
#### Mask Attack
```shell
hashcat -a 3 -m 0 md5_mask_example_hash -1 01 'ILFREIGHT?l?l?l?l?l20?1?d'
```
### Hybrid Mode
#### Creating Hybrid Hash
```shell
echo -n 'football1$' | md5sum | tr -d " -" > hybrid_hash
```
#### Hybrid Attack using Wordlists
```shell
hashcat -a 6 -m 0 hybrid_hash /home/kali/Desktop/SecLists/Passwords/Leaked-Databases/rockyou.txt '?d?s'
```
#### Creating another Hybrid Hash
```shell
echo -n '2015football' | md5sum | tr -d " -" > hybrid_hash_prefix 
```
#### Hybrid Attack using Wordlists with Masks
```shell
hashcat -a 7 -m 0 hybrid_hash_prefix -1 01 '20?1?d' /home/kali/Desktop/SecLists/Passwords/Leaked-Databases/rockyou.txt
```
# Crunch
```shell
crunch <minimum length> <maximum length> <charset> -t <pattern> -o <output file>
```
#### Generate Word List
```shell
crunch 4 8 -o wordlist
```
#### Create Word List using Pattern
```shell
crunch 17 17 -t ILFREIGHT201%@@@@ -o wordlist
```
#### Specified Repetition
```shell
crunch 12 12 -t 10031998@@@@ -d 1 -o wordlist
```
# CUPP
```shell
python3 cupp.py -i
```
# KWPROCESSOR
```shell
git clone https://github.com/hashcat/kwprocessor
cd kwprocessor
make
```

```shell
kwp -s 1 basechars/full.base keymaps/en-us.keymap  routes/2-to-10-max-3-direction-changes.route
```
# Princeprocessor
```shell
wget https://github.com/hashcat/princeprocessor/releases/download/v0.22/princeprocessor-0.22.7z
7z x princeprocessor-0.22.7z
cd princeprocessor-0.22
./pp64.bin -h
```
#### Wordlist
```txt
dog
cat
ball
```
#### Princeprocessor - Generated Wordlist
```txt
dog
cat
ball
dogdog
catdog
dogcat
catcat
dogball
catball
balldog
ballcat
ballball
dogdogdog
catdogdog
dogcatdog
catcatdog
dogdogcat
<SNIP>
```
#### Find the Number of Combinations
```shell
./pp64.bin --keyspace < words
```
#### Forming Wordlist
```shell
./pp64.bin -o wordlist.txt < words
```
#### Password Length Limits
```shell
./pp64.bin --pw-min=10 --pw-max=25 -o wordlist.txt < words
```
#### Specifying Elements
```shell
./pp64.bin --elem-cnt-min=3 -o wordlist.txt < words
```
# CeWL
```shell
cewl -d <depth to spider> -m <minimum word length> -w <output wordlist> <url of website>
```

```shell
cewl -d 5 -m 8 -e http://inlanefreight.com/blog -w wordlist.txt
```
# Previously Cracked Passwords
```shell
cut -d: -f 2- ~/hashcat.potfile
```
# Hashcat-utils
```git
https://github.com/hashcat/hashcat-utils.git
```
# Rule Creation
#### Rules
```shell
c so0 si1 se3 ss5 sa@ $2 $0 $1 $9
```

```shell
echo 'c so0 si1 se3 ss5 sa@ $2 $0 $1 $9' > rule.txt
```
#### Store the Password in a File
```shell
echo 'password_ilfreight' > test.txt
```
#### Hashcat - Debugging Rules
```shell
hashcat -r rule.txt test.txt --stdout
```
`P@55w0rd_1lfr31ght2019`
#### Generate SHA1 Hash
```shell
echo -n 'St@r5h1p2019' | sha1sum | awk '{print $1}' | tee hash
```
#### Hashcat - Cracking Passwords Using Wordlists and Rules
```shell
hashcat -a 0 -m 100 hash /home/kali/Desktop/SecLists/Passwords/Leaked-Databases/rockyou.txt -r rule.txt
```
#### Hashcat - Default Rules
```shell
ls -l /usr/share/hashcat/rules/
```
#### Hashcat - Generate Random Rules
```shell-session
hashcat -a 0 -m 100 -g 1000 hash /home/kali/Desktop/SecLists/Passwords/Leaked-Databases/rockyou.txt
```
# Cracking Common Hashes
## Example 1 - Database Dumps
#### Generate SHA1 Hashes
```shell
for i in $(cat <words.txt>); do echo -n $i | sha1sum | tr -d ' -';done
```

```shell
hashcat -m 100 SHA1_hashes /home/kali/Desktop/SecLists/Passwords/Leaked-Databases/rockyou.txt
```
## Example 2 - Linux Shadow File
`/etc/shadow`
#### Root Password in Ubuntu Linux
```shell
root:$6$tOA0cyybhb/Hr7DN$htr2vffCWiPGnyFOicJiXJVMbk1muPORR.eRGYfBYUnNPUjWABGPFiphjIjJC5xPfFUASIbVKDAHS3vTW1qU.1:18285:0:99999:7:::
```

```shell
hashcat -m 1800 shadow.txt /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt
```
## Example 3 - Common Active Directory Password Hash Types
### NTLM
#### Python3 - Hashlib
```shell
python3
```
```python
import hashlib,binascii
hash = hashlib.new('md4', "Password01".encode('utf-16le')).digest()
print (binascii.hexlify(hash))
```
`b'7100a909c7ff05b266af3c42ec058c33'`
#### Hashcat - Cracking NTLM Hashes
```shell
hashcat -a 0 -m 1000 ntlm_example /home/kali/Desktop/SecLists/Passwords/Leaked-Databases/rockyou.txt
```
### NTLMv2
```hash
sqladmin::INLANEFREIGHT:f54d6f198a7a47d4:7FECABAE13101DAAA20F1B09F7F7A4EA:0101000000000000C0653150DE09D20126F3F71DF13C1FD8000000000200080053004D004200330001001E00570049004E002D00500052004800340039003200520051004100460056000400140053004D00420033002E006C006F00630061006C0003003400570049004E002D00500052004800340039003200520051004100460056002E0053004D00420033002E006C006F00630061006C000500140053004D00420033002E006C006F00630061006C0007000800C0653150DE09D201060004000200000008003000300000000000000000000000003000001A67637962F2B7BF297745E6074934196D5F4371B6BA3E796F2997306FD4C1C00A001000000000000000000000000000000000000900280063006900660073002F003100390032002E003100360038002E003100390035002E00310037003000000000000000000000000000
```
#### Hashcat - Cracking NTLMv2 Hashes
```shell
hashcat -a 0 -m 5600 inlanefreight_ntlmv2 /home/kali/Desktop/SecLists/Passwords/Leaked-Databases/rockyou.txt
```
# Cracking Miscellaneous Files & Hashes
## JohnTheRipper
```shell
sudo git clone https://github.com/magnumripper/JohnTheRipper.git
cd JohnTheRipper/src
sudo ./configure && make
```
## Example 1 - Cracking Password Protected MS Office Documents

| Mode | Target          |
|------|-----------------|
| 9400 | MS Office 2007  |
| 9500 | MS Office 2010  |
| 9600 | MS Office 2013  |
### Extract Hash
```python
python office2john.py hashcat_Word_example.docx > office_hash
```
### Hashcat - Cracking MS Office Passwords
```shell
hashcat -m 9600 office_hash /home/kali/Desktop/SecLists/Passwords/Leaked-Databases/rockyou.txt
```
## Example 2 - Cracking Password Protected Zip Files
#### Set Password for a ZIP File
```shell
zip --password zippyzippy blueprints.zip dummy.pdf 
```
#### Extract Hash
```shell
zip2john ~/Desktop/HTB/Academy/Cracking\ with\ Hashcat/blueprints.zip 
```
#### Hashcat - Cracking ZIP Files
```shell
hashcat -a 0 -m 17200 pdf_hash_to_crack /home/kali/Desktop/SecLists/Passwords/Leaked-Databases/rockyou.txt
```
## Example 3 - Cracking Password Protected KeePass Files
#### Extract Hash
```shell
python keepass2john.py Master.kdbx 
```
#### Hashcat - Cracking KeePass Files
```shell
hashcat -a 0 -m 13400 keepass_hash /home/kali/Desktop/SecLists/Passwords/Leaked-Databases/rockyou.txt
```
## Example 4 - Cracking Protected PDF Files
#### Extract Hash
```shell
python pdf2john.py inventory.pdf | awk -F":" '{ print $2}'
```
#### Hashcat - Cracking PDF Files
```shell-session
hashcat -a 0 -m 10500 pdf_hash /home/kali/Desktop/SecLists/Passwords/Leaked-Databases/rockyou.txt
```
# Cracking Wireless (WPA/WPA2) Handshakes with Hashcat
## Cracking MIC
#### Hashcat-Utils
```shell
git clone https://github.com/hashcat/hashcat-utils.git
cd hashcat-utils/src
make
```
#### Cap2hccapx - Syntax
```shell
./cap2hccapx.bin 
```
#### Cap2hccapx - Convert To Crackable File
```shell
./cap2hccapx.bin corp_capture1-01.cap mic_to_crack.hccapx
```
#### Hashcat - Cracking WPA Handshakes
```shell
hashcat -a 0 -m 22000 mic_to_crack.hccapx /home/kali/Desktop/SecLists/Passwords/Leaked-Databases/rockyou.txt
```
## Cracking PMKID
#### Hcxpcaptool - Extract PMKID
```shell
hcxpcaptool -z pmkidhash_corp cracking_pmkid.cap 
```
#### PMKID-Hash
```shell
cat pmkidhash_corp 
```
#### Hashcat - Cracking PMKID
```shell
hashcat -a 0 -m 22000 pmkidhash_corp /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt
```
#### Hcxtools
```shell
git clone https://github.com/ZerBea/hcxtools.git
cd hcxtools
make && make install
```
```shell
hcxpcapngtool cracking_pmkid.cap -o pmkidhash_corp2
```

