### Enumerate Users:

```python
#!/usr/bin/python
import socket
import sys
if len(sys.argv) != 2:
print "Usage: vrfy.py <username>"
sys.exit(0)
# Create a Socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# Connect to the Server
connect = s.connect(('10.11.1.217',25))
# Receive the banner
banner = s.recv(1024)
print banner
# VRFY a user
s.send('VRFY ' + sys.argv[1] + '\r\n')
result = s.recv(1024)
print result
# Close the
```

AUTH NTLM V2

https://medium.com/swlh/internal-information-disclosure-using-hidden-ntlm-authentication-18de17675666

### Annymous Login and Send mail

```shell
telnet 10.11.1.101 25
HELO mail.thinc.local
MAIL FROM: admin@thinc.local
RCPT TO: AGH@thinc.local
DATA # To notify the console that you want to write mail. END THE DATA WITH `.`
SUBJECT: Test message
# DATA DATA DATA DATA DATA => dont forget to end you data with `.`
QUIT # close connection

```


### Importtant CMD 

```shell
VRFY VictemUsername
```

sendEmail -t layla@VICTIM -f catty@apply.com -s 192.168.237.55 -u 'job application' -m 'htpp://192.168.119.237/cv'