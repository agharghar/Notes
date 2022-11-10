

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt --format=NT
```

### unshadow

```bash
unshadow passwd-file.txt shadow-file.txt
unshadow passwd-file.txt shadow-file.txt > unshadowed.txt
```

```bash
john --rules --wordlist=/usr/share/wordlists/rockyou.txt unshadowed.txt
```