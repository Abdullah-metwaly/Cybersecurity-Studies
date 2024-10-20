
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
