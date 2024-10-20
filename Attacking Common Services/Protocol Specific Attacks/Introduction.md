

## Server Message Block (SMB)

```C
New-PSDrive -Name "N" -Root "\\192.168.220.129\Finance" -PSProvider "FileSystem" -Credential $cred
```

```c
C:\htb> net use n: \\192.168.220.129\Finance
```

```c
C:\htb> net use n: \\192.168.220.129\Finance /user:plaintext Password123
```

```c
C:\htb> dir n: /a-d /s /b | find /c ":\"
```

#smb #net #share #cmd 

|**Syntax**|**Description**|
|---|---|
|`dir`|Application|
|`n:`|Directory or drive to search|
|`/a-d`|`/a` is the attribute and `-d` means not directories|
|`/s`|Displays files in a specified directory and all subdirectories|
|`/b`|Uses bare format (no heading information or summary)|


```c
c:\htb> dir n:\*cred* /s /b
```


```c
c:\htb> findstr /s /i cred n:\*.*
```

#findstr #cmd #hunting 

#### Windows PowerShell

```
PS C:\htb> Get-ChildItem \\192.168.220.129\Finance\
```

```
PS C:\htb> New-PSDrive -Name "N" -Root "\\192.168.220.129\Finance" -PSProvider "FileSystem"
```

```
PS C:\htb> $username = 'plaintext'
PS C:\htb> $password = 'Password123'
PS C:\htb> $secpassword = ConvertTo-SecureString $password -AsPlainText -Force
PS C:\htb> $cred = New-Object System.Management.Automation.PSCredential $username, $secpassword
PS C:\htb> New-PSDrive -Name "N" -Root "\\192.168.220.129\Finance" -PSProvider "FileSystem" -Credential $cred
```

```
PS C:\htb> N:
PS N:\> (Get-ChildItem -File -Recurse | Measure-Object).Count
```

```
PS C:\htb> Get-ChildItem -Recurse -Path N:\ -Include *cred* -File
```

```
PS C:\htb> Get-ChildItem -Recurse -Path N:\ | Select-String "cred" -List
```

#powershell #smb #share 
#### Linux - Mount

```
$ sudo mkdir /mnt/Finance
```

```
$ sudo mount -t cifs -o username=plaintext,password=Password123,domain=. //192.168.220.129/Finance /mnt/Finance
```

		 with a credential file 

```
$ mount -t cifs //192.168.220.129/Finance /mnt/Finance -o credentials=/path/credentialfile
```

```
sudo mount -t nfs IP:share /tmp/mount/ -nolock
```
#mount 

```
/usr/sbin/showmount -e $ip
```
#showmount 

		 credentialfile 

```
username=plaintext
password=Password123
domain=.
```

```
$ find /mnt/Finance/ -name *cred*
```

```
$ grep -rn /mnt/Finance/ -ie cred
```

#mount #grep #smb 

## Command Line Utilities

#### MSSQL

>To interact with [MSSQL (Microsoft SQL Server)](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) with Linux we can use [sqsh](https://en.wikipedia.org/wiki/Sqsh) or [sqlcmd](https://docs.microsoft.com/en-us/sql/tools/sqlcmd-utility) if you are using Windows. `Sqsh` is much more than a friendly prompt. It is intended to provide much of the functionality provided by a command shell, such as variables, aliasing, redirection, pipes, back-grounding, job control, history, command substitution, and dynamic configuration. We can start an interactive SQL session as follows:

```
$ sqsh -S 10.129.20.13 -U username -P Password123
```

```
$ mysql -u username -pPassword123 -h 10.129.20.13
```

#linux #sql #mysql

```c
C:\htb> sqlcmd -S 10.129.20.13 -U username -P Password123
```

```c
C:\htb> mysql.exe -u username -pPassword123 -h 10.129.20.13
```

#windows #sql #mysql 


#### Tools to Interact with Common Services

|**SMB**|**FTP**|**Email**|**Databases**|
|---|---|---|---|
|[smbclient](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html)|[ftp](https://linux.die.net/man/1/ftp)|[Thunderbird](https://www.thunderbird.net/en-US/)|[mssql-cli](https://github.com/dbcli/mssql-cli)|
|[CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec)|[lftp](https://lftp.yar.ru/)|[Claws](https://www.claws-mail.org/)|[mycli](https://github.com/dbcli/mycli)|
|[SMBMap](https://github.com/ShawnDEvans/smbmap)|[ncftp](https://www.ncftp.com/)|[Geary](https://wiki.gnome.org/Apps/Geary)|[mssqlclient.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/mssqlclient.py)|
|[Impacket](https://github.com/SecureAuthCorp/impacket)|[filezilla](https://filezilla-project.org/)|[MailSpring](https://getmailspring.com)|[dbeaver](https://github.com/dbeaver/dbeaver)|
|[psexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/psexec.py)|[crossftp](http://www.crossftp.com/)|[mutt](http://www.mutt.org/)|[MySQL Workbench](https://dev.mysql.com/downloads/workbench/)|
|[smbexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/smbexec.py)||[mailutils](https://mailutils.org/)|[SQL Server Management Studio or SSMS](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms)|
|||[sendEmail](https://github.com/mogaal/sendemail)||
|||[swaks](http://www.jetmore.org/john/code/swaks/)||
|||[sendmail](https://en.wikipedia.org/wiki/Sendmail)||

#network_tools


#### The Concept of Attacks


![[attack_concept2.png]]

#image


#### Log4j

![[log4jattack.png]]

#log4j
