```bash
# -t : search in title, -w: show URL
searchsploit afd windows -w -t
```


```bash
#list all urls:
searchsploit afd windows -w -t | grep http | cut -f 2 -d "|"
```

```bash
#download all exploit
for e in $(searchsploit afd windows -w -t | grep http | cut -f 2 -d "|");do exp_name=$(echo $e | cut -d "/" -f 5) && url=$(echo $e | sed 's/exploits/raw/') && wget -q --no-check-certificate $url -O $exp_name; done
```


