1. [[Attacking FTP]]
2. [[Attacking SMB]]
3. [[Attacking SQL Databases]]
4. [[Attacking RDP]]
5. [[Attacking DNS]]
6. [[Attacking Email Services]]



## Enumeration - FTP

`Nmap` default scripts `-sC` includes the [ftp-anon](https://nmap.org/nsedoc/scripts/ftp-anon.html) Nmap script which checks if a FTP server allows anonymous logins. The version enumeration flag `-sV` provides interesting information about FTP services, such as the FTP banner, which often includes the version name. We can use the `ftp` client or `nc` to interact with the FTP service. By default, FTP runs on TCP port 21.


## Enumeration - SMB

Depending on the SMB implementation and the operating system, we will get different information using `Nmap`. Keep in mind that when targetting Windows OS, version information is usually not included as part of the Nmap scan results. Instead, Nmap will try to guess the OS version. However, we will often need other scans to identify if the target is vulnerable to a particular exploit. We will cover searching for known vulnerabilities later in this section. For now, let's scan ports 139 and 445 TCP.


## Enumeration - SQL

By default, MSSQL uses ports `TCP/1433` and `UDP/1434`, and MySQL uses `TCP/3306`. However, when MSSQL operates in a "hidden" mode, it uses the `TCP/2433` port. We can use `Nmap`'s default scripts `-sC` option to enumerate database services on a target system:


## Enumeration - RDP

By default, RDP uses port `TCP/3389`. Using `Nmap`, we can identify the available RDP service on the target host:


## Enumeration - DNS

DNS holds interesting information for an organization. As discussed in the Domain Information section in the [Footprinting module](https://academy.hackthebox.com/course/preview/footprinting), we can understand how a company operates and the services they provide, as well as third-party service providers like emails.

The Nmap `-sC` (default scripts) and `-sV` (version scan) options can be used to perform initial enumeration against the target DNS servers: 53


## Enumeration - SMTP

|**Port**|**Service**|
|---|---|
|`TCP/25`|SMTP Unencrypted|
|`TCP/143`|IMAP4 Unencrypted|
|`TCP/110`|POP3 Unencrypted|
|`TCP/465`|SMTP Encrypted|
|`TCP/993`|IMAP4 Encrypted|
|`TCP/995`|POP3 Encrypted|


