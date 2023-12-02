
>A `mail server` (sometimes also referred to as an email server) is a server that handles and delivers email over a network, usually over the Internet. A mail server can receive emails from a client device and send them to other mail servers. A mail server can also deliver emails to a client device. A client is usually the device where we read our emails (computers, smartphones, etc.).
>>
>>When we press the `Send` button in our email application (email client), the program establishes a connection to an `SMTP` server on the network or Internet. The name `SMTP` stands for Simple Mail Transfer Protocol, and it is a protocol for delivering emails from clients to servers and from servers to other servers.
>>>
>>>When we download emails to our email application, it will connect to a `POP3` or `IMAP4` server on the Internet, which allows the user to save messages in a server mailbox and download them periodically.
>>>>
>>>>By default, `POP3` clients remove downloaded messages from the email server. This behavior makes it difficult to access email on multiple devices since downloaded messages are stored on the local computer. However, we can typically configure a `POP3` client to keep copies of downloaded messages on the server.
>>>>On the other hand, by default, `IMAP4` clients do not remove downloaded messages from the email server. This behavior makes it easy to access email messages from multiple devices. Let's see how we can target mail servers.


![[SMTP-IMAP-1.png]]

---

```
$ host -t MX hackthebox.eu
```

```
$ host -t MX microsoft.com
```

```
$ dig mx plaintext.do | grep "MX" | grep -v ";"
```

```
$ dig mx inlanefreight.com | grep "MX" | grep -v ";"
```

```
$ host -t A mail1.inlanefreight.htb
```
#host #dig #mx #grep 

|**Port**|**Service**|
|---|---|
|`TCP/25`|SMTP Unencrypted|
|`TCP/143`|IMAP4 Unencrypted|
|`TCP/110`|POP3 Unencrypted|
|`TCP/465`|SMTP Encrypted|
|`TCP/993`|IMAP4 Encrypted|
|`TCP/995`|POP3 Encrypted|

```
$ sudo nmap -Pn -sV -sC -p25,143,110,465,993,995 10.129.14.128
```

```
$ telnet 10.10.110.20 25
```

```
$ telnet 10.10.110.20 110
```

```
$ smtp-user-enum -M RCPT -U userlist.txt -D inlanefreight.htb -t 10.129.203.7
```

```
$ python3 o365spray.py --validate --domain msplaintext.xyz
```

```
$ python3 o365spray.py --enum -U users.txt --domain msplaintext.xyz  
```

```
$ hydra -L users.txt -p 'Company01!' -f 10.10.110.20 pop3
```

```
$ python3 o365spray.py --spray -U usersfound.txt -p 'March2022!' --count 1 --lockout 1 --domain msplaintext.xyz
```

#nmap #smtp #imap #pop3 #telnet #smtp-user-enum #python #o365spray #hydra #login #passwordspray #bruteforce 






```
$ nmap -p25 -Pn --script smtp-open-relay 10.10.11.213
```
#nma #smtp #script 

```
$ swaks --from notifications@inlanefreight.com --to employees@inlanefreight.com --header 'Subject: Company Notification' --body 'Hi All, we want to hear from you! Please complete the following survey. http://mycustomphishinglink.com/' --server 10.10.11.213
```
#swaks 

```
$ curl -k 'imaps://<FQDN/IP>' --user <user>:<password> -v 
```

```
$ openssl s_client -connect <FQDN/IP>:imap
```

```
$ openssl s_client -connect <FQDN/IP>:pop
```
#curl #openssl #imap #pop3 