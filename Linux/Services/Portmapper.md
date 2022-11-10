### Enumurating

We can use NSE scripts like rpcinfo to find services that may have registered with rpcbind:

```shell
nmap -sV -p 111 --script=rpcinfo 10.11.1.1-254
```

```shell
rpcinfo -s 10.11.1.221
rpcinfo -p 10.11.1.221
```
