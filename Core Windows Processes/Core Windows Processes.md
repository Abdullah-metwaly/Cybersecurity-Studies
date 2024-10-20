
Image Path: C:\Windows\system32\ntoskrnl.exe(NT OS Kernel)
Parent Process: System Idle Process (0)

Technically this is correct. You may notice that Process Hacker confirms this is legit (Verified) Microsoft Windows. 

What is unusual behavior for this process?

- A parent process (aside from System Idle Process (0))
- Multiple instances of System. (Should only be one instance) 
- A different PID. (Remember that the PID will always be PID 4)
- Not running in Session 0

#### smss.exe

**Image Path**:  %SystemRoot%\System32\smss.exe
This process also known as the windows session manager, responsible for creating new sessions. It is the first user-mode process started by the kernel

This process starts the kernel and user modes of the Windows subsystem. The subsystem includes win32k.sys (kernel mode), winsrv.dll (user mode), and csrss.exe (user mode). 

smss.exe starts csrss.exe (Windows subsystem) and wininit.exe in session 0, an isolated Windows session for the operating system,  and crss.exe and winlogon.exe for session 1, which is the user session. The child instance creates child instances in new sessions, done by smss.exe copying itself into the new session and self-terminating. 

#### csrss.exe

starts from the smss.exe in two separate sessions 0 and 1, one session with wininit.exe and another with winlogon.exe. It manage console window, handle thread creations, and some parts of the gui creations. Runs in user mode but works with kernel level. 

#### wininit.exe 

Responsible for initializing crucial system services during the boot up launching service.exe, lsass.exe, and lsaiso.exe within session 0. It's a critical windows process and its child processes. 

#### service.exe

It's main responsibility to handle system services: loading, interacting, and starting/ending service. It also maintain a database to query from powershell using sc.exe. 

When a user logs into a machine successfully, this process is responsible for setting the value of the Last Known Good control set (Last Known Good Configuration), `HKLM\System\Select\LastKnownGood`, to that of the CurrentControlSet.

It's a parent process for multiple other processes e.g. (svchost.exe, spoolsv.exe, msmpeng.exe and, dllhost.exe )

#### svchost.exe
it's one crucial windows process that handles windows services in one place instance of it. It groups service related to the same one svchost.exe to save windows consumptions. Failure in this process can lead to restart it and it's not user interactive like app processes. It's normal to see multiple process in the task manager for svchost.exe.  

#### Lsass.exe 
Windows process that's responsible for enforcing security policy on the system it verifies user logon to a windows computer or server, handle password changes, and create access token. It's logging in security logs. 

#### winlogo.exe
The **Windows Logon**, **winlogon.exe**, is responsible for handling the **Secure Attention Sequence** (SAS). It is the ALT+CTRL+DELETE key combination users press to enter their username & password.

smss.exe launches this process along with a copy of csrss.exe within Session 1.

#### explorer.exe

The last process we'll look at is **Windows Explorer**, **explorer.exe**. This process gives the user access to their folders and files. It also provides functionality for other features, such as the Start Menu and Taskbar.