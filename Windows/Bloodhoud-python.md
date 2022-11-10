```shell
sudo docker run -v ${PWD}:/bloodhound-data -it bloodhound

bloodhound-python -ns 10.10.92.166 -d domain.com -dc <FQDN : if not FQDN => not work> -c all -u 'username' -p 'password'
```

Use [[Jq]] to extract users list => brute force after