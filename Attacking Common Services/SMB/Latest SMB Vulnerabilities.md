- One recent significant vulnerability that affected the SMB protocol was called [SMBGhost](https://arista.my.site.com/AristaCommunity/s/article/SMBGhost-Wormable-Vulnerability-Analysis-CVE-2020-0796) with the [CVE-2020-0796](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2020-0796). 
- The vulnerability consisted of a compression mechanism of the version SMB v3.1.1 which made Windows 10 versions 1903 and 1909 vulnerable to attack by an unauthenticated attacker. 
- The vulnerability allowed the attacker to gain remote code execution (`RCE`) and full access to the remote target system.
#### The Concept of Attacks
![[smb-latest.png]]