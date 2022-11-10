### Man

```shell
curl 10.11.1.71 -s -L | grep "title\|href" | sed -e 's/^[[:space:]]*//'
curl 10.11.1.71 -s -L | grep "img\|svg" | sed -e 's/^[[:space:]]*//'
curl 10.11.1.71 -s -L | html2text -width '99' | uniq
```

```shell
gobuster dir  -u http://http://10.1.1.95   -w /usr/share/seclists/Discovery/Web-Content/big.txt   -s '200,204,301,302,307,403,500' -e


gobuster dir  -u http://10.1.1.95/   -w /usr/share/seclists/Discovery/Web-Content/CGIs.txt   -s '200,204,403,500' -e

```

```shell
ffuf -w /usr/share/wordlists/dirb/big.txt -u https://10.2.2.150/FUZZ
```

```bash
wfuzz --hc 404 -z file,/usr/share/wordlists/dirb/big.txt  -p 127.0.0.1:8080 "http://10.11.1.8/FUZZ"
```

```shell
feroxbuster -u https://10.2.2.150/ -t 10 -w /root/.config/AutoRecon/wordlists/dirbuster.txt -x "txt,html,php,asp,aspx,jsp" -v -k -n -q -e -o "/home/kali/Documents/PWK/lab/public/10.11.1.71/results/10.11.1.71/scans/tcp80/tcp_80_http_feroxbuster_dirbuster.txt"
```

```shell
dirb http://10.5.5.25:8080/ -w
```