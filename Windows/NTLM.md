## Enumerate for protocol uses NTLM For AUTH

```shell
nmap -sS -v --script=*-ntlm-info --script-timeout=60s example.com
```