```
ssh username@<IP>
```
```
ssh -i /path/to/id_rsa username@remote_ip
```

## SSH Downloads
#### Enabling the SSH Server
```shell
sudo systemctl enable ssh
```
#### Starting the SSH Server
```shell
sudo systemctl start ssh
```
#### Checking for SSH Listening Port
```shell
netstat -lnpt
```
## SCP
#### Linux - Downloading Files Using SCP
```shell
scp plaintext@192.168.49.128:/root/myroot.txt . 
```
#### Linux - Uploading Files Using SCP
```shell
scp /etc/passwd htb-student@10.129.86.90:/home/htb-student/
```
