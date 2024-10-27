Sysmon, a tool used to monitor and log events on Windows, is commonly used by enterprises as part of their monitoring and logging solutions. Part of the Windows Sysinternals package, Sysmon is similar to Windows Event Logs with further detail and granular control.


**Event ID 1: Process Creation**

```
<RuleGroup name="" groupRelation="or">   
	<ProcessCreate onmatch="exclude">     <CommandLine condition="is">C:\Windows\system32\svchost.exe -k appmodel -p -s camsvc
	</CommandLine>   
	</ProcessCreate>  
	 </RuleGroup>`
```


**Event ID 3: Network Connection**

```
<RuleGroup name="" groupRelation="or">   
	<NetworkConnect onmatch="include">     
	<Image condition="image">nmap.exe</Image>     
	<DestinationPort name="Alert,Metasploit" condition="is">4444</DestinationPort>   </NetworkConnect>  
 </RuleGroup>`
```

**Event ID 7: Image Loaded**

```
<RuleGroup name="" groupRelation="or">   
		<ImageLoad onmatch="include">     
		<ImageLoaded condition="contains">\Temp\</ImageLoaded>   
		</ImageLoad>   
		</RuleGroup>`
```


**Event ID 8: CreateRemoteThread**

The CreateRemoteThread Event ID will monitor for processes injecting code into other processes. The CreateRemoteThread function is used for legitimate tasks and applications. However, it could be used by malware to hide malicious activity. This event will use the SourceImage, TargetImage, StartAddress, and StartFunction XML tags.


```
<RuleGroup name="" groupRelation="or">   
	<CreateRemoteThread onmatch="include">     
	<StartAddress name="Alert,Cobalt Strike" condition="end with">0B80</StartAddress>     
	<SourceImage condition="contains">\</SourceImage>   
	</CreateRemoteThread>   
	</RuleGroup>   `
```


**Event ID 11: File Created**


```
<RuleGroup name="" groupRelation="or">   
	<FileCreate onmatch="include">     
	<TargetFilename name="Alert,Ransomware" condition="contains">HELP_TO_SAVE_FILES</TargetFilename>   
	</FileCreate>   
</RuleGroup>`
```

**Event ID 12 / 13 / 14: Registry Event

```
<RuleGroup name="" groupRelation="or">  
	<RegistryEvent onmatch="include">   
	<TargetObject name="T1484" condition="contains">Windows\System\Scripts</TargetObject>   
	</RegistryEvent>  
</RuleGroup>`
```



**Event ID 15: FileCreateStreamHash

This event will look for any files created in an alternate data stream. This is a common technique used by adversaries to hide malware. This event uses TargetFilename XML tags.

```
<RuleGroup name="" groupRelation="or">   
	<FileCreateStreamHash onmatch="include">     
	<TargetFilename condition="end with">.hta</TargetFilename>   </FileCreateStreamHash>   
</RuleGroup>`
```

**Event ID 22: DNS Event

```
<RuleGroup name="" groupRelation="or">   
	<DnsQuery onmatch="exclude">     
	<QueryName condition="end with">.microsoft.com</QueryName>   
	</DnsQuery>   
</RuleGroup>`
```



**installing sysmon

```powershell
Sysmon.exe -accepteula -i ..\Configuration\swift.xml
```


	Sysmon hunting
```powershell
Get-WinEvent -Path C:\Users\..\....evtx -FilterXPath '*/System/EventID=3 and */EventData/Data[@Name="DestinationPort"] and */EventData/Data=4444'
```

	Mimikatz
```powershell
Get-WinEvent -Path C:\Users\..\....evtx -FilterXPath '*/System/EventID=10 and */EventData/Data[@Name="TargetImage"] and */EventData/Data="C:\Windows\system32\lsass.exe"'
```

	 Malware
```powershell
Get-WinEvent -Path C:\Users\..\....evtx -FilterXPath '*/System/EventID=3 and */EventData/Data[@Name="DestinationPort"] and */EventData/Data=8080'
```

	 Technique evasion
```powershell
Get-WinEvent -Path C:\Users\...\...evtx -FilterXPath '*/System/EventID=8'
```

