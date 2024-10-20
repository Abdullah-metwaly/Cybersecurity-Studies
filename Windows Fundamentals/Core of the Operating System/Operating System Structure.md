| Directory | Function |
| ---- | ---- |
| Perflogs | Can hold Windows performance logs but is empty by default. |
| Program Files | On 32-bit systems, all 16-bit and 32-bit programs are installed here. On 64-bit systems, only 64-bit programs are installed here. |
| Program Files (x86) | 32-bit and 16-bit programs are installed here on 64-bit editions of Windows. |
| ProgramData | This is a hidden folder that contains data that is essential for certain installed programs to run. This data is accessible by the program no matter what user is running it. |
| Users | This folder contains user profiles for each user that logs onto the system and contains the two folders Public and Default. |
| Default | This is the default user profile template for all created users. Whenever a new user is added to the system, their profile is based on the Default profile. |
| Public | This folder is intended for computer users to share files and is accessible to all users by default. This folder is shared over the network by default but requires a valid network account to access. |
| AppData | Per user application data and settings are stored in a hidden user subfolder (i.e., cliff.moore\AppData). Each of these folders contains three subfolders. The Roaming folder contains machine-independent data that should follow the user's profile, such as custom dictionaries. The Local folder is specific to the computer itself and is never synchronized across the network. LocalLow is similar to the Local folder, but it has a lower data integrity level. Therefore it can be used, for example, by a web browser set to protected or safe mode. |
| Windows | The majority of the files required for the Windows operating system are contained here. |
| System, System32, SysWOW64 | Contains all DLLs required for the core features of Windows and the Windows API. The operating system searches these folders any time a program asks to load a DLL without specifying an absolute path. |
| WinSxS | The Windows Component Store contains a copy of all Windows components, updates, and service packs. |
|  |  |
```c
C:\htb> dir c:\ /a
```

```
tree "c:\Program Files (x86)\VMware"
```

```
tree c:\ /f | more
```
#tree

# File System

## Permissions

The NTFS file system has many basic and advanced permissions. Some of the key permission types are:

|Permission Type|Description|
|---|---|
|Full Control|Allows reading, writing, changing, deleting of files/folders.|
|Modify|Allows reading, writing, and deleting of files/folders.|
|List Folder Contents|Allows for viewing and listing folders and subfolders as well as executing files. Folders only inherit this permission.|
|Read and Execute|Allows for viewing and listing files and subfolders as well as executing files. Files and folders inherit this permission.|
|Write|Allows for adding files to folders and subfolders and writing to a file.|
|Read|Allows for viewing and listing of folders and subfolders and viewing a file's contents.|
|Traverse Folder|This allows or denies the ability to move through folders to reach other files or folders. For example, a user may not have permission to list the directory contents or view files in the documents or web apps directory in this example c:\users\bsmith\documents\webapps\backups\backup_02042020.zip but with Traverse Folder permissions applied, they can access the backup archive.|


- `(CI)`: container inherit
- `(OI)`: object inherit
- `(IO)`: inherit only
- `(NP)`: do not propagate inherit
- `(I)`: permission inherited from parent container

Basic access permissions are as follows:

- `F` : full access
- `D` :  delete access
- `N` :  no access
- `M` :  modify access
- `RX` :  read and execute access
- `R` :  read-only access
- `W` :  write-only access

```c
C:\htb> icacls c:\Users
```

```c
C:\htb> icacls c:\users /grant joe:f
```

```c
C:\htb> >icacls c:\users
```
#icacls

# NTFS vs. Share Permissions
![smb diagram](https://academy.hackthebox.com/storage/modules/49/smb_diagram.png)
#### Using smbclient to Connect to the Share

```
smbclient -L IPaddressOfTarget -U htb-student
```
#smbclient 
```
sudo mount -t cifs -o username=htb-student,password=Academy_WinFun! //ipaddoftarget/"Company Data" /home/user/Desktop/
```
#mount #cifs
```
sudo apt-get install cifs-utils
```

```c
C:\Users\htb-student> net share
```
#net 

# Windows Services & Processes

|Service|Description|
|---|---|
|smss.exe|Session Manager SubSystem. Responsible for handling sessions on the system.|
|csrss.exe|Client Server Runtime Process. The user-mode portion of the Windows subsystem.|
|wininit.exe|Starts the Wininit file .ini file that lists all of the changes to be made to Windows when the computer is restarted after installing a program.|
|logonui.exe|Used for facilitating user login into a PC|
|lsass.exe|The Local Security Authentication Server verifies the validity of user logons to a PC or server. It generates the process responsible for authenticating users for the Winlogon service.|
|services.exe|Manages the operation of starting and stopping services.|
|winlogon.exe|Responsible for handling the secure attention sequence, loading a user profile on logon, and locking the computer when a screensaver is running.|
|System|A background system process that runs the Windows kernel.|
|svchost.exe with RPCSS|Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Remote Procedure Call (RPC) Service (RPCSS).|
|svchost.exe with Dcom/PnP|Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Distributed Component Object Model (DCOM) and Plug and Play (PnP) services.|<br><br><br>
https://en.wikipedia.org/wiki/List_of_Microsoft_Windows_components#Services


```c
C:\htb> \\live.sysinternals.com\tools\procdump.exe -accepteula
```
#sysinternals 

```powershell-session
PS C:\htb> Get-Service | ? {$_.Status -eq "Running"} | select -First 2 |fl
```
#powershell 

```
Get-Service | Where-Object { $_.Status -eq "Running" -and $_.DisplayName -like "*pdf*" }
```
#powershell 

# Service Permissions

