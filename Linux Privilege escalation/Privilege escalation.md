
find suid files
```
find / -perm /4000 2>/dev/null
```


- `/` - start looking recursivly from the root directory
- `-perm` - specify file permission we are looking for
- `/4000` - list all files with the SUID bit active
- `2>/dev/nul` - remove all errors from STDOUT (screen)



**Note:** If you get an error saying `Unable to negotiate with <IP> port 22: no matching how to key type found. Their offer: ssh-rsa, ssh-dss` this is because OpenSSH have deprecated ssh-rsa. Add `-oHostKeyAlgorithms=+ssh-rsa` to your command to connect.


		 list up running services

```
systemctl list-units --type=service --state=running
```
#systemctl 



```
ssh -i root_key -oPubkeyAcceptedKeyTypes=+ssh-rsa -oHostKeyAlgorithms=+ssh-rsa root@10.10.156.54
```
#ssh 

		 important bash scripting link 
https://devhints.io/bash
