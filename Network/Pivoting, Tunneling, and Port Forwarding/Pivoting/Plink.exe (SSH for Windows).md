- [Plink](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), short for PuTTY Link, is a Windows command-line SSH tool that comes as a part of the PuTTY package when installed. 
- Similar to SSH, Plink can also be used to create dynamic port forwards and SOCKS proxies.
![[splink.png]]
#### Plink.exe
```cmd
plink -ssh -D 9050 ubuntu@10.129.15.50
```
#### Proxifier
- [Proxifier](https://www.proxifier.com) can be used to start a SOCKS tunnel via the SSH session we created. 
- Proxifier is a Windows tool that creates a tunneled network for desktop client applications and allows it to operate through a SOCKS or HTTPS proxy and allows for proxy chaining. 
- It is possible to create a profile where we can provide the configuration for our SOCKS server started by Plink on port 9050.
