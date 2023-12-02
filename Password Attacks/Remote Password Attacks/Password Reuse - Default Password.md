## Credential Stuffing

There are various databases that keep a running list of known default credentials. One of them is the [DefaultCreds-Cheat-Sheet](https://github.com/ihebski/DefaultCreds-cheat-sheet). Here is a small excerpt from the entire table of this cheat sheet:
All these default passwords can be used with both modules of ([[Network Services]] and [[Password Mutations]]) to brute force passwords with #hydra 

|**Product/Vendor**|**Username**|**Password**|
|---|---|---|
|Zyxel (ssh)|zyfwp|PrOw!aN_fXp|
|APC UPS (web)|apc|apc|
|Weblogic (web)|system|manager|
|Weblogic (web)|system|manager|
|Weblogic (web)|weblogic|weblogic1|
|Weblogic (web)|WEBLOGIC|WEBLOGIC|
|Weblogic (web)|PUBLIC|PUBLIC|
|Weblogic (web)|EXAMPLES|EXAMPLES|
|Weblogic (web)|weblogic|weblogic|
|Weblogic (web)|system|password|
|Weblogic (web)|weblogic|welcome(1)|
|Weblogic (web)|system|welcome(1)|
|Weblogic (web)|operator|weblogic|
|Weblogic (web)|operator|password|
|Weblogic (web)|system|Passw0rd|
|Weblogic (web)|monitor|password|
|Kanboard (web)|admin|admin|
|Vectr (web)|admin|11_ThisIsTheFirstPassword_11|
|Caldera (web)|admin|admin|
|Dlink (web)|admin|admin|
|Dlink (web)|1234|1234|
|Dlink (web)|root|12345|
|Dlink (web)|root|root|
|JioFiber|admin|jiocentrum|
|GigaFiber|admin|jiocentrum|
|Kali linux (OS)|kali|kali|
|F5|admin|admin|
|F5|root|default|
|F5|support||
|...|...|...|

### Credential Stuffing - Hydra Syntax

```
$ hydra -C <user_pass.list> <protocol>://<IP>
```

#password_mutations #hydra 


Besides the default credentials for applications, some lists offer them for routers. One of these lists can be found [here](https://www.softwaretestinghelp.com/default-router-username-and-password-list/). It is much less likely that the default credentials for routers are left unchanged. Since these are the central interfaces for networks, administrators typically pay much closer attention to hardening them. Nevertheless, it is still possible that a router is overlooked or is currently only being used in the internal network for test purposes, which we can then exploit for further attacks.


|**Router Brand**|**Default IP Address**|**Default Username**|**Default Password**|
|---|---|---|---|
|3Com|http://192.168.1.1|admin|Admin|
|Belkin|http://192.168.2.1|admin|admin|
|BenQ|http://192.168.1.1|admin|Admin|
|D-Link|http://192.168.0.1|admin|Admin|
|Digicom|http://192.168.1.254|admin|Michelangelo|
|Linksys|http://192.168.1.1|admin|Admin|
|Netgear|http://192.168.0.1|admin|password|

