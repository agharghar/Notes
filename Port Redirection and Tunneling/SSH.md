###  SSH Local Port Forwarding
```bash
sudo ssh -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" -N -L 0.0.0.0:8080:192.168.237.52:80 student@192.168.237.52
# -N : no command
```

### SSH Remote Port Forwarding

```shell
ssh -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no"  -N -R 192.168.119.167:13306:10.5.5.11:3306  -R 1122:10.5.5.11:22 kali@192.168.119.167
# -N : no command
```

```shell
ssh -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" 
-f -N -R 1080 -i /home/sean/.ssh/id_rsa kali@192.168.119.155

#-R 1080 => SOCKS On local host
```
-N : no command


### SSH Dynamic Port Forwarding

```shell
sudo ssh -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no"  -N -D 127.0.0.1:1080 sean@10.11.1.251
# -N : no command
# 127.0.0.1:8080 => SOCKS5
```