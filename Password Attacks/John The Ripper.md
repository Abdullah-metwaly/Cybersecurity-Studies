
 ```
 -john hash --format=Whirlpool --wordlist=/usr/share/wordlists/rockyou.txt
```
#john 

```
john --list=format
```

```
wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py
```
#hash-id

```
john --show --format=sha512crypt hash 
```

```
john --single --format=raw-md5 hash
```

```
john --wordlist=[path to wordlist] --rule=PoloPassword [path to file]
```

```
cp /opt/john/rar2john /usr/local/bin
```

```
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
```


