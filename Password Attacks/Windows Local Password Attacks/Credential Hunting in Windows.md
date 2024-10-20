>Once we have access to a target Windows machine through the GUI or CLI, we can significantly benefit from incorporating credential hunting into our approach. `Credential Hunting` is the process of performing detailed searches across the file system and through various applications to discover credentials. To understand this concept, let's place ourselves in a scenario. We have gained access to an IT admin's Windows 10 workstation through RDP.



#### Running Lazagne All 

```c
C:\Users\bob\Desktop> start lazagne.exe all
```

#### Using findstr

```c
C:\> findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml *.git *.ps1 *.yml
```
