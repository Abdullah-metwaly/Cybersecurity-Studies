
>Linux-based distributions can use many different authentication mechanisms. One of the most commonly used and standard mechanisms is [Pluggable Authentication Modules](http://www.linux-pam.org/Linux-PAM-html/Linux-PAM_SAG.html) (`PAM`). The modules used for this are called `pam_unix.so` or `pam_unix2.so` and are located in `/usr/lib/x86_x64-linux-gnu/security/` in Debian based distributions. These modules manage user information, authentication, sessions, current passwords, and old passwords. For example, if we want to change the password of our account on the Linux system with `passwd`, PAM is called, which takes the appropriate precautions and stores and handles the information accordingly.

> The `pam_unix.so` standard module for management uses standardized API calls from the system libraries and files to update the account information. The standard files that are read, managed, and updated are `/etc/passwd` and `/etc/shadow`. PAM also has many other service modules, such as LDAP, mount, or Kerberos.

We can take this module as an extension of [[Credential Hunting in Linux]]


---

#### Passwd Format

|`cry0l1t3`|`:`|`x`|`:`|`1000`|`:`|`1000`|`:`|`cry0l1t3,,,`|`:`|`/home/cry0l1t3`|`:`|`/bin/bash`|
|---|---|---|---|---|---|---|---|---|---|---|---|---|
|Login name||Password info||UID||GUID||Full name/comments||Home directory||Shell|

```
root:x:0:0:root:/root:/bin/bash
```

```
$ awk -F':' '{ print $1}' /etc/passwd
```

```
$ cut -d: -f1 /etc/passwd
```

```
$ getent passwd | cut -d: -f1
```

```
$ getent passwd username
```

```
$ grep "^UID_MIN" /etc/login.defs
```


#passwd 
#### Root without Password

```
$ head -n 1 /etc/passwd

root::0:0:root:/root:/bin/bash

```


#root
## Shadow File

#### Shadow Format

|`cry0l1t3`|`:`|`$6$wBRzy$...SNIP...x9cDWUxW1`|`:`|`18937`|`:`|`0`|`:`|`99999`|`:`|`7`|`:`|`:`|`:`|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|Username||Encrypted password||Last PW change||Min. PW age||Max. PW age||Warning period|Inactivity period|Expiration date|Unused|

```
$ sudo cat /etc/shadow
```

#shadow 
#### Algorithm Types

- `$1$` – MD5
- `$2a$` – Blowfish
- `$2y$` – Eksblowfish
- `$5$` – SHA-256
- `$6$` – SHA-512

#hashes
#### Reading /etc/security/opasswd

```
$ sudo cat /etc/security/opasswd
```

#opasswd

## Cracking Linux Credentials

```
$ sudo cp /etc/passwd /tmp/passwd.bak 
$ sudo cp /etc/shadow /tmp/shadow.bak 
$ unshadow /tmp/passwd.bak /tmp/shadow.bak > /tmp/unshadowed.hashes
```

```
hashcat -m 1800 -a 0 /tmp/unshadowed.hashes rockyou.txt -o /tmp/unshadowed.cracked
```

```
$ cat md5-hashes.list
```

```
$ hashcat -m 500 -a 0 md5-hashes.list rockyou.txt
```

#unshadow #hashcat #md5 #passwd #shadow 

```
$ awk -F':' -v "min=${l##UID_MIN}" -v "max=${l1##UID_MAX}" '{ if ( $3 >= min && $3 <= max  && $7 != "/sbin/nologin" ) print $0 }' "/etc/passwd"
```

```
awk -F':' -v "min=${l##UID_MIN}" -v "max=${l1##UID_MAX}" '{ if ( !($3 >= min && $3 <= max  && $7 != "/sbin/nologin")) print $0 }' "/etc/passwd"
```
