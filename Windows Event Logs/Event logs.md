"Combining log file entries from multiple sources can also be useful. This approach, in combination with statistical analysis, may yield correlations between seemingly unrelated events on different servers."


Windows event logs are not text files that can be viewed using a text editor. However, the raw data can be translated into XML using the windows API. The events in these log files are stored in a proprietary binary format with a .evt or .evtx extension. Evtx file extensions resides in C:\Windows\System32\winevt\Logs

Elements of Windows Event Log

**System Logs:** Events associated with the operating system segments. Hardware changes, device drivers, system changes, and activates related to the device. 
**System Logs:** logon and logoff activates on a device. Excellent location for investigating authorization attempts.
**Application Logs:** Application related events on the system. Application errors, events, and warning. 
**Directory Service Events:** Active Directory changes and activities are recorded in these logs, mainly on domain controllers. 
**File Replication Service Events:** Windows Servers changes are recorded here during the sharing of group policy, logon script.
**DNS Event Logs:** DNS servers use these logs to record domain events and to map out
**Custom Logs:** Events are logged by applications that require custom data storage. This allows applications to control the log size or attach other parameters, such as ACLs, for security purposes. 


There are three main ways of accessing these event logs within a Windows system:

1. **Event Viewer** (GUI-based application)
2. **Wevtutil.exe** (command-line tool)
3. **Get-WinEvent** (PowerShell cmdlet)



#### XPATH Queries


Below are some xpath queries to be use from powershell commands 

```
PS C:\Users\Administrator> Get-WinEvent -LogName Application -FilterXPath '*'

Get-WinEvent -LogName Application -FilterXPath '*/System/EventID=100'


Get-WinEvent -LogName Application -FilterXPath '*/System/EventID=101 and */System/Provider[@Name="WLMS"]'

Get-WinEvent -LogName Application -FilterXPath '*/System/Provider[@Name="WLMS"] and */System/TimeCreated[@SystemTime="2020-12-15T01:09:08.940277500Z"]'
```

![[Pasted image 20241023201253.png]]



If we want to query from the EventData section in XML we can do it something like below

```
Get-WinEvent -LogName Security -FilterXPath '*/EventData/Data[@Name="TargetUserName"]="System"'

Get-WinEvent -LogName Security -FilterXPath '*/EventData/Data[@Name="TargetUserName"]="Sam" and */System/EventID=4720'

```

![[Pasted image 20241023203913.png]]

querying from custom log saved on the pc

```
Get-WinEvent -Path .\Desktop\merged.evtx -FilterXPath '*/System/EventID=4104' -Oldest -MaxEvents 1 | Format-List

Get-WinEvent -Path .\Desktop\merged.evtx -FilterXPath '*/System/Execution[@ProcessID="6620"]' | Format-List


```

