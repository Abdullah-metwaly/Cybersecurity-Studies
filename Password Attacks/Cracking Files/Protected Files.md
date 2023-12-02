>The use of file encryption is often still lacking in `private` and `business` matters. Even today, emails containing job applications, account statements, or contracts are often sent unencrypted. This is grossly negligent and, in many cases, even punishable by law. For example, GDPR demands the requirement for encrypted storage and transmission of personal data in the European Union. Especially in business cases, this is quite different for emails. Nowadays, it is pretty common to communicate `confidential` topics or send `sensitive` data `by email`. However, emails are not much more secure than postcards, which can be intercepted if the attacker is positioned correctly.

>More and more companies are increasing their IT security precautions and infrastructure through training courses and security awareness seminars. As a result, it is becoming increasingly common for company employees to encrypt/encode sensitive files. Nevertheless, even these can be cracked and read with the right choice of lists and tools. In many cases, `symmetric encryption` like `AES-256` is used to securely store individual files or folders. Here, the `same key` is used to encrypt and decrypt a file.

[[Protected Files]]


#### Hunting for Files

```
$ for ext in $(echo ".xls .xls* .xltx .csv .od* .doc .doc* .pdf .pot .pot* .pp*");do echo -e "\nFile extension: " $ext; find / -name *$ext 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```

#bash #bashscripting 

#### Hunting for SSH Keys

```
$ grep -rnw "PRIVATE KEY" /* 2>/dev/null | grep ":1"
```

#hunting #grep #ssh 

#### Encrypted SSH Keys

```
$ cat /home/cry0l1t3/.ssh/SSH.private
```

## Cracking with John

```
$ locate *2john*
```

```shell-session
/usr/bin/bitlocker2john
/usr/bin/dmg2john
/usr/bin/gpg2john
/usr/bin/hccap2john
/usr/bin/keepass2john
/usr/bin/putty2john
/usr/bin/racf2john
/usr/bin/rar2john
/usr/bin/uaf2john
/usr/bin/vncpcap2john
/usr/bin/wlanhcx2john
/usr/bin/wpapcap2john
/usr/bin/zip2john
/usr/share/john/1password2john.py
/usr/share/john/7z2john.pl
/usr/share/john/DPAPImk2john.py
/usr/share/john/adxcsouf2john.py
/usr/share/john/aem2john.py
/usr/share/john/aix2john.pl
/usr/share/john/aix2john.py
/usr/share/john/andotp2john.py
/usr/share/john/androidbackup2john.py
```

```
$ ssh2john.py SSH.private > ssh.hash
```

```
$ cat ssh.hash 
```

```
$ john --wordlist=rockyou.txt ssh.hash
```

```
$ john ssh.hash --show
```


		Cracking Microsoft Office Documents

```
$ office2john.py Protected.docx > protected-docx.hash
```

```
$ cat protected-docx.hash
```

```
$ john --wordlist=rockyou.txt protected-docx.hash
```

```
$ john protected-docx.hash --show
```


		 Cracking PDFs 

```
$ pdf2john.py PDF.pdf > pdf.hash
```

```
$ cat pdf.hash 
```

```
$ john --wordlist=rockyou.txt pdf.hash
```

```
$ john pdf.hash --show
```

