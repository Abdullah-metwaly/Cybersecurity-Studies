>Besides standalone files, there is also another format of files that can contain not only data, such as an Office document or a PDF, but also other files within them. This format is called an `archive` or `compressed file` that can be protected with a password if necessary.

>Let us assume an employee's role in an administrative company and imagine that our customer wants to summarize analysis in different formats, such as Excel, PDF, Word, and a corresponding presentation. One solution would be to send these files individually, but if we extend this example to a large company dealing with several projects running simultaneously, this type of file transfer can become cumbersome and lead to individual files being lost. In these cases, employees often rely on archives, which allow them to split all the necessary files in a structured way according to the projects (often in subfolders), summarize them, and pack them into a single file.

---

#### Download All File Extensions

```
$ curl -s https://fileinfo.com/filetypes/compressed | html2text | awk '{print tolower($1)}' | grep "\." | tee -a compressed_ext.txt
```

#curl #awk 

#### Using zip2john

```
$ zip2john ZIP.zip > zip.hash
```

```
$ cat zip.hash
```

```
$ john --wordlist=rockyou.txt zip.hash
```

```
$ john zip.hash --show
```

#john 

## Cracking OpenSSL Encrypted Archives

```
$ file GZIP.gzip 
```

```
$ for i in $(cat rockyou.txt);do openssl enc -aes-256-cbc -d -in GZIP.gzip -k $i 2>/dev/null| tar
```

#gzip #openssl #bash 

## Cracking BitLocker Encrypted Drives

```
$ bitlocker2john -i Backup.vhd > backup.hashes
```

```
$ grep "bitlocker\$0" backup.hashes > backup.hash
```

```
$ cat backup.hash
```


```
$ hashcat -m 22100 backup.hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt -o backup.cracked
```


```
$ cat backup.cracked 
```

#vhd #hashcat #bitlocker 