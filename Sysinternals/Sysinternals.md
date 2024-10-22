

What are the tools known as **Sysinternals**?

The Sysinternals tools is a compilation of over 70+ Windows-based tools. Each of the tools falls into one of the following categories:

- File and Disk Utilities
- Networking Utilities
- Process Utilities
- Security Utilities
- System Information
- Miscellaneous

The Sysinternals tools and its website (sysinternals.com) were created by Mark Russinovich in the late '90s, along Bryce Cogswell under the company Wininternals Software.  

In 2006, Microsoft acquired Wininternals Software, and Mark Russinovich joined Microsoft. Today he is the CTO of Microsoft Azure.

We can use the sysintenals live without downloading the files from website live.sysinternals.com/tool-name

to be able to communicate with the tool web client service must be enabled and started before using the tool live. 

The WebDAV protocol is what allows a local machine to access a remote machine running a WebDAV share and perform actions in it.

```
get-service webclient 
start-service webclient 
```

Also, **Network Discovery** needs to be enabled as well. This setting can be enabled in the **Network and Sharing Center**.


- WebClient service is running
- Network Discovery is enabled


#### Steams

NTFS offers a feature to hide data/metadate/entire file in the main file without seeing the content. Files has primary stream of data which is normally visible when file is opened. This stream of data is unnamed data stream. Hidden data stream is named Alternate Data Stream ADS it's not visible to the user. PowerShell can print this and 3rd parties executables. 

	to check for hidden streams
```
streams C:\path\to\file\file.txt -accepteula
```



#### TCPView 

"**TCPView** is a Windows program that will show you detailed listings of all TCP and UDP endpoints on your system, including the local and remote addresses and state of TCP connections. On Windows Server 2008, Vista, and XP, TCPView also reports the name of the process that owns the endpoint. TCPView provides a more informative and conveniently presented subset of the Netstat program that ships with Windows. The TCPView download includes Tcpvcon, a command-line version with the same functionality." (**official definition**)


There's also a built-in windows utility resource monitor 'rasmon' to shows also the monitoring of processes running per category. 

![[resource-monitor.png]]


#### Autoruns

Autoruns is an essential tool for users who want to have detailed control over their system’s startup processes. Its ability to show a comprehensive list of auto-starting programs and services makes it a valuable resource for system optimization and security. If you're looking to enhance your Windows experience or troubleshoot issues, Autoruns provides the insight needed to manage startup behavior effectively.

#### ProcDump

"**ProcDump** is a command-line utility whose primary purpose is monitoring an application for CPU spikes and generating crash dumps during a spike that an administrator or developer can use to determine the cause of the spike." (official definition)


#### Process Explorer

"The **Process Explorer** display consists of two sub-windows. The top window always shows a list of the currently active processes, including the names of their owning accounts, whereas the information displayed in the bottom window depends on the mode that Process Explorer is in: if it is in handle mode you'll see the handles that the process selected in the top window has opened; if Process Explorer is in DLL mode you'll see the DLLs and memory-mapped files that the process has loaded." (official definition)


#### Process Monitor

"Process Monitor is an advanced monitoring tool for Windows that shows real-time file system, Registry and process/thread activity. It combines the features of two legacy Sysinternals utilities, Filemon and Regmon, and adds an extensive list of enhancements including rich and non-destructive filtering, comprehensive event properties such as session IDs and user names, reliable process information, full thread stacks with integrated symbol support for each operation, simultaneous logging to a file, and much more. Its uniquely powerful features will make Process Monitor a core utility in your system troubleshooting and malware hunting toolkit." (**official definition**)

#### PsExec

"**PsExec** is a light-weight telnet-replacement that lets you execute processes on other systems, complete with full interactivity for console applications, without having to manually install client software. PsExec's most powerful uses include launching interactive command-prompts on remote systems and remote-enabling tools like IpConfig that otherwise do not have the ability to show information about remote systems." (official definition)


#### Sysmon

"System Monitor (Sysmon) is a Windows system service and device driver that, once installed on a system, remains resident across system reboots to monitor and log system activity to the Windows event log. It provides detailed information about process creations, network connections, and changes to file creation time. By collecting the events it generates using Windows Event Collection or SIEM agents and subsequently analyzing them, you can identify malicious or anomalous activity and understand how intruders and malware operate on your network." (**official definition**)

#### WinObj

"**WinObj** is a 32-bit Windows NT program that uses the native Windows NT API (provided by NTDLL.DLL) to access and display information on the NT Object Manager's name space." (**official definition**)

#### Bginfo

"It automatically displays relevant information about a Windows computer on the desktop's background, such as the computer name, IP address, service pack version, and more." (**official definition**)

#### Strings

"Strings just scans the file you pass it for UNICODE (or ASCII) strings of a default length of 3 or more UNICODE (or ASCII) characters. Note that it works under Windows 95 as well." (official definition)
