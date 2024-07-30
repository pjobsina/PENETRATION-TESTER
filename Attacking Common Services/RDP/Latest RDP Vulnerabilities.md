- In 2019, a critical vulnerability was published in the RDP (`TCP/3389`) service that also led to remote code execution (`RCE`) with the identifier [CVE-2019-0708](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2019-0708). 
- This vulnerability is known as `BlueKeep`. 
- It does not require prior access to the system to exploit the service for our purposes. 
- However, the exploitation of this vulnerability led and still leads to many malware or ransomware attacks.
## The Concept of the Attack
- The vulnerability is also based, as with SMB, on manipulated requests sent to the targeted service. 
- However, the dangerous thing here is that the vulnerability does not require user authentication to be triggered. 
- Instead, the vulnerability occurs after initializing the connection when basic settings are exchanged between client and server. 
- This is known as a [Use-After-Free](https://cwe.mitre.org/data/definitions/416.html) (`UAF`) technique that uses freed memory to execute arbitrary code.