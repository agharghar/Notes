### ALL Ports
```bash
sudo nmap -v -p- -Pn -n --min-rate=5000 -oA enum/all-tcp-ports 10.11.1.21
```

### Default SC and Version
```bash
sudo nmap -v -p21,80,135,139,445,3389,5985,47001,49664,49666,49667,49669,49671 -Pn -n -sC -sV -T4 -oA enum/service-tcp 10.11.1.21
```