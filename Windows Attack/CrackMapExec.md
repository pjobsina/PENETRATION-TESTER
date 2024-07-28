```shell
sudo apt-get -y install crackmapexec
```

```shell
crackmapexec <proto> <target-IP> -u <user or userlist> -p <password or passwordlist>
```

```shell
crackmapexec winrm 10.129.42.197 -u user.list -p password.list
```
###### SMB
```shell
crackmapexec smb 10.129.42.197 -u "user" -p "password" --shares
```