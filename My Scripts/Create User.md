```shell
@echo off

title add user

echo Adding user...
net user agharghar agharghar /add
net localgroup Administrators agharghar /ADD
net localgroup "Remote Desktop Users" agharghar /add

echo Enabling RDP...
reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f

echo #############################
echo User Added
echo #############################
net users
```