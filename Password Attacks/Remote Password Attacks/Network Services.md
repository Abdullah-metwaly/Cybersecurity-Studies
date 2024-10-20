# CrackMapExec Usage 

CrackMapExec (a.k.a CME) is a post-exploitation tool that _helps automate assessing the security of large Active Directory networks_.

> Installation of the tool
---

```
$ sudo apt-get install crackmapexec
```



> The general format for using CrackMapExec is as follows:
---
```
$ crackmapexec <proto> <target-IP> -u <user or userlist> -p <password or passwordlist>
```

```
$ crackmapexec winrm 10.129.42.197 -u user.list -p password.list

WINRM       10.129.42.197   5985   NONE             [*] None (name:10.129.42.197) (domain:None)
WINRM       10.129.42.197   5985   NONE             [*] http://10.129.42.197:5985/wsman
WINRM       10.129.42.197   5985   NONE             [+] None\user:password (Pwn3d!)
```

```
$ crackmapexec smb 10.129.42.197 -u "user" -p "password" --shares
```

#crackmapexec #network_service
# Evil-WinRM Usage

A handy tool that we can use to communicate with the WinRM service is [Evil-WinRM](https://github.com/Hackplayers/evil-winrm), which allows us to communicate with the WinRM service efficiently.

> Installation of the tool
---

```
$ sudo apt-get install evil-winrm 
```



> The general format for using CrackMapExec is as follows:
---
```
$ evil-winrm -i <target-IP> -u <username> -p <password>
```

#evil-winrm #network_service
# SSH

[Secure Shell](https://www.ssh.com/academy/ssh/protocol) (`SSH`) is a more secure way to connect to a remote host to execute system commands or transfer files from a host to a server. The SSH server runs on `TCP port 22` by default, to which we can connect using an SSH client. This service uses three different cryptography operations/methods: `symmetric` encryption, `asymmetric` encryption, and `hashing`.
### Hydra - SSH

>  We can use a tool such as `Hydra` to brute force SSH. This is covered in-depth in the [Login Brute Forcing](https://academy.hackthebox.com/course/preview/login-brute-forcing/introduction-to-brute-forcing) module.


```
$ hydra -L user.list -P password.list ssh://10.129.42.197
```

```
hydra -t 16 -l administrator -P /usr/share/wordlists/rockyou.txt -vV 10.10.148.172 ssh
```
#hydra #ssh 

To log in to the system via the SSH protocol, we can use the OpenSSH client, which is available by default on most Linux distributions. 

```
$ ssh user@10.129.42.197
```

#ssh #network_service #hydra 
# Remote Desktop Protocol (RDP)

Microsoft's [Remote Desktop Protocol](https://docs.microsoft.com/en-us/troubleshoot/windows-server/remote/understanding-remote-desktop-protocol) (`RDP`) is a network protocol that allows remote access to Windows systems via `TCP port 3389` by default. RDP provides both users and administrators/support staff with remote access to Windows hosts within an organization. The Remote Desktop Protocol defines two participants for a connection: a so-called terminal server, on which the actual work takes place, and a terminal client, via which the terminal server is remotely controlled. In addition to the exchange of image, sound, keyboard, and pointing device, the RDP can also print documents of the terminal server on a printer connected to the terminal client or allow access to storage media available there. Technically, the RDP is an application layer protocol in the IP stack and can use TCP and UDP for data transmission. The protocol is used by various official Microsoft apps, but it is also used in some third-party solutions.

### Hydra - RDP

> We can also use `Hydra` to perform RDP bruteforcing.

```
$ hydra -L user.list -P password.list rdp://10.129.42.197
```

### xFreeRDP

 > xfreerdp is an X11 Remote Desktop Protocol (RDP) client which is part of the FreeRDP project. An RDP server is built-in to many editions of Windows. Alternative servers included xrdp and VRDP (VirtualBox). 


```
$ xfreerdp /v:<target-IP> /u:<username> /p:<password>
```

#rdp #network_service #hydra 

# SMB

[Server Message Block](https://docs.microsoft.com/en-us/windows/win32/fileio/microsoft-smb-protocol-and-cifs-protocol-overview) (`SMB`) is a protocol responsible for transferring data between a client and a server in local area networks. It is used to implement file and directory sharing and printing services in Windows networks. SMB is often referred to as a file system, but it is not. SMB can be compared to `NFS` for Unix and Linux for providing drives on local networks.

SMB is also known as [Common Internet File System](https://cifs.com/) (`CIFS`). It is part of the SMB protocol and enables universal remote connection of multiple platforms such as Windows, Linux, or macOS. In addition, we will often encounter [Samba](https://wiki.samba.org/index.php/Main_Page), which is an open-source implementation of the above functions. For SMB, we can also use `hydra` again to try different usernames in combination with different passwords.

### Hydra - SMB

> We can also use `Hydra` to perform SMB bruteforcing.

```
$ hydra -L user.list -P password.list smb://10.129.42.197
```

> usage of smbclient

```
$ smbclient -U user \\\\10.129.42.197\\SHARENAME
```


#smb  #network_service #hydra  #smbclient
