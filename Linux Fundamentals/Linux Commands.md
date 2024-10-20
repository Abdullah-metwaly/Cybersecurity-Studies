

```
cat /etc/passwd | cut -d: -f1
```
#cat #passwd 
```
$ awk -F’:’ ‘{ print $1}’ /etc/passwd
```
#awk 
```
cat /etc/os-release
```
#release
```
cat /etc/update-motd/00-header
```
#motd 

		Encrypt
```
gpg --symmetric --cipher-algo CIPHER message.txt
```

		Adding the armor tag to create ASCII output
```
gpg --armor --symmetric --cipher-algo CIPHER message.txt
```

		Decrypt
```
gpg --output original_message.txt --decrypt message.gpg
```
#gpg

		Encrypt
```
openssl aes-256-cbc -e -in message.txt -out encrypted_message
```

```
openssl aes-256-cbc -pbkdf2 -iter 10000 -e -in message.txt -out encrypted_message
```
#encrypt
			Dencrypt 
```
openssl aes-256-cbc -d -in encrypted_message -out original_message.txt
```

```
openssl aes-256-cbc -pbkdf2 -iter 10000 -d -in encrypted_message -out original_message.txt
```
#openssl #decrypt


## RSA

```
openssl genrsa -out private-key.pem 2048
```

```
openssl rsa -in private-key.pem -pubout -out public-key.pem
```

```
openssl rsa -in private-key.pem -text -noout
```

```
openssl pkeyutl -encrypt -in plaintext.txt -out ciphertext -inkey public-key.pem -pubin
```

```
openssl pkeyutl -decrypt -in ciphertext -inkey private-key.pem -out decrypted.txt
```
#openssl #decrypt #encrypt #rsa


## Diffie-Hellman

```
openssl dhparam -out dhparams.pem 2048
```

```
openssl dhparam -in dhparams.pem -text -noout
```


## Hashing

```
sha256sum *
```

```
hexdump text1.txt -C
```


```
hmac256 s!Kr37 message.txt
```

```
sha256hmac message.txt --key s!Kr37
```
#sha256hmac #hmac #hashing

## PKI 

		generate a certificate signing request using the command
```
openssl req -new -nodes -newkey rsa:4096 -keyout key.pem -out cert.csr
```


		command will generate a self-signed certificate.
```
openssl req -x509 -newkey -nodes rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 365
```

		the following command to view your certificate:

```
openssl x509 -in cert.pem -text
```
#openssl #PKI 

## Finding commands


		Find files based on filename
```
find [directory path] -type f -name [filename]
```

		Find Directory based on directory name
```
find [directory path] -type d -name [filename]
```

		Find files based on size
```
find [directory path] -type f -size [size]
```

		Find files based on username
```
find [directory path] -type f -user [username]
```

		Find files based on group name
```
find [directory path] -type f -group [group name]
```

		Find files modified after a specific date
```
find [directory path] -type f -newermt '[date and time]'

find / -type f -newermt '6/30/2020 0:00:00'
```

		Find files modified after a specific date
```
find [directory path] -type f -newermt '[date and time]'
```

		Find files based on date modified
```
find [directory path] -type f -newermt [start date range] ! -newermt [end date range]
```

		Find files based on date accessed
```
find [directory path] -type f -newerat [start date range] ! -newerat [end date range]
```

		Find files with a specific keyword
```
grep -iRl [directory path/keyword]
```

		read the manual for the find command
```
man find
```
#find 

## Working with files

		copy file/folder
```
cp ssh.conf /home/newfolder
```

		move file/folder
```
mv ssh.conf /home/newfolder
```

		move multiple files/folders simultaneously
```
mv logs.txt keys.conf script.py -t /home/savedWork
```

		Move all files from current directory into another directory
```
mv * [directory to move files to]
```

		rename files/folder
```
mv [current filename] [new filename]
```

		create a file
```
touch newfile.txt
```

		create a folder
```
mkdir newFolder
```

		open file for editing
```
nano keys.conf
```

		output contents of file
```
cat [filename]
```

		upload file to a remote machine
```
scp [filename] [username]@[IP of remote machine ]:/[directory to upload to]

scp example.txt john@192.168.100.123:/home/john/
```


		run an bash script program
```
./[name of script]
```

		open a file for reading/editing
```
nano [filename]
```

```
mv -- -logs -newlogs
```


#nano #mv #cat #mkdir #scp #touch #cp


## Encryption/decryption


		how to encrypt a file
```
gpg --cipher-algo [encryption type] [encryption method] [file to encrypt]
```
#gpg 

		how to decrypt a file 
```
gpg secret.txt.gpg
```

```
haiti --no-color --short ent.txt
```

```
haiti -e ent.txt 
```
#haiti

```
dpkg -l | grep ^ii | wc -l
```
#dpkg

```
find /etc/ -name shadow 2> stderr.txt 1> stdout.txt
```

![image](https://academy.hackthebox.com/storage/modules/18/find4.png)

#stderr #stdout #find 


```
cat /etc/passwd | grep -v "false\|nologin" | cut -d":" -f1
```
#cut
```
cat /etc/passwd | grep -v "false\|nologin"
```
#grep 
```
more /etc/passwd
```
#more
```
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " "
```
#tr
```
 cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | column -t
```
#column
```
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}'
```
#awk 
```
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}' | sed 's/bin/HTB/g'
```
#sed
```
cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}' | wc -l
```
#tr #sed #awk #grep #wc 


```
grep -E "(my|false)" /etc/passwd
```
#grep 


# Package Management

```
cat /etc/apt/sources.list.d/parrot.list
```

```
apt-cache search impacket
```

```
apt-cache show impacket-scripts
```

```
apt list --installed
```

```
sudo apt install impacket-scripts -y
```
#apt #apt-cache 

```
mkdir ~/nishang/ && git clone https://github.com/samratashok/nishang.git ~/nishang
```
#mkdir 

```
wget http://archive.ubuntu.com/ubuntu/pool/main/s/strace/strace_4.21-1ubuntu1_amd64.deb
```
#wget 

```
sudo dpkg -i strace_4.21-1ubuntu1_amd64.deb 
```
#dpkg 


# Service and Process Management


```
systemctl start ssh
```

```
systemctl status ssh
```

```
systemctl enable ssh
```
#systemctl 


```
ps -aux | grep ssh
```
#ps 

```
systemctl list-units --type=service
```
#systemctl 

```
journalctl -u ssh.service --no-pager
```
#journalctl 

```
kill 9 <PID> 
```
#kill

```
ping -c 10 www.hackthebox.eu
```
```
ping -c 10 www.hackthebox.eu &
```
#ping 

```
jobs
```
#jobs

```
bg
```
#bg

```
fg 1
```
#fg

```
echo '1'; echo '2'; echo '3'
```

```
echo '1'; ls MISSING_FILE; echo '3'
```

```
echo '1' && ls MISSING_FILE && echo '3'
```



# Task Scheduling

## Systemd

Systemd is a service used in Linux systems such as Ubuntu, Redhat Linux, and Solaris to start processes and scripts at a specific time. With it, we can set up processes and scripts to run at a specific time or time interval and can also specify specific events and triggers that will trigger a specific task. To do this, we need to take some steps and precautions before our scripts or processes are automatically executed by the system.

1. Create a timer
2. Create a service
3. Activate the timer

```
sudo mkdir /etc/systemd/system/mytimer.timer.d
```

```
sudo vim /etc/systemd/system/mytimer.timer
```
#sudo #mkdir #vim 


Next, we need to create a script that configures the timer. The script must contain the following options: "Unit", "Timer" and "Install". The "Unit" option specifies a description for the timer. The "Timer" option specifies when to start the timer and when to activate it. Finally, the "Install" option specifies where to install the timer.

```txt
[Unit]
Description=My Timer

[Timer]
OnBootSec=3min
OnUnitActiveSec=1hour

[Install]
WantedBy=timers.target
```


Here it depends on how we want to use our script. For example, if we want to run our script only once after the system boot, we should use `OnBootSec` setting in `Timer`. However, if we want our script to run regularly, then we should use the `OnUnitActiveSec` to have the system run the script at regular intervals. Next, we need to create our `service`.

```
sudo vim /etc/systemd/system/mytimer.service
```


```txt
[Unit]
Description=My Service

[Service]
ExecStart=/full/path/to/my/script.sh

[Install]
WantedBy=multi-user.target
```

		we have to let `systemd` read the folders again to include the changes.
```
sudo systemctl daemon-reload
```

```
sudo systemctl start mytimer.service
```

```
sudo systemctl enable mytimer.service
```
#systemctl 

```
systemctl show -p Type syslog.service
```
#systemctl #type

# Network Services

## NFS 

```
systemctl status nfs-kernel-server
```


		Target machine create NFS share 
```
mkdir nfs_sharing
echo '/home/cry0l1t3/nfs_sharing hostname(rw,sync,no_root_squash)' >> /etc/exports
cat /etc/exports | grep -v "#"
```

		 mount the NFS on local machine
```
mkdir ~/target_nfs
mount 10.129.12.17:/home/john/dev_scripts ~/target_nfs
tree ~/target_nfs
```

So we have mounted the NFS share (`dev_scripts`) from our target (`10.129.12.17`) locally to our system in the mount point `target_nfs` over the network and can view the contents just as if we were on the target system. There are even some methods that can be used in specific cases to escalate our privileges on the remote system using NFS.


```
systemd-resolve --interface breachad --set-dns $THMDCIP --set-domain za.tryhackme.com
```
#systemd-resolve

## Web Server

```
sudo apt install apache2 -y
```
#apache 
```
python3 -m http.server --directory /home/cry0l1t3/target_files
```
#python3 
```
sudo apt install openvpn -y
```
#openvpn 

```
http-server -p 8080
```
#npm

```
php -S 127.0.0.1:8080
```
#php


```
sudo apt install rsync -y
```

		Rsync - Backup a local Directory to our Backup-Server
```
rsync -av /path/to/mydirectory user@backup_server:/path/to/backup/directory
```
#rsync 

			Rsync - Restore our Backup
```
rsync -av user@remote_host:/path/to/backup/directory /path/to/mydirectory
```
#rsync  

			 Secure Transfer of our Backup
```
rsync -avz -e ssh /path/to/mydirectory user@backup_server:/path/to/backup/directory
```
#rsync 

## Auto-Synchronization

#### RSYNC_Backup.sh

```bash
#!/bin/bash

rsync -avz -e ssh /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

```shell
chmod +x RSYNC_Backup.sh
```
#rsync 
#### Auto-Sync - Crontab
```
0 * * * * /path/to/RSYNC_Backup.sh
```
#crontab 



```
sudo mount /dev/sdb1 /mnt/usb
```
#mount 

```
sudo umount /mnt/usb
```
#unmount 

```
lsof | grep cry0l1t3
```
#lsof


#### Install Docker-Engine

```bash
#!/bin/bash

# Preparation
sudo apt update -y
sudo apt install ca-certificates curl gnupg lsb-release -y
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt update -y
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

# Add user htb-student to the Docker group
sudo usermod -aG docker htb-student
echo '[!] You need to log out and log back in for the group changes to take effect.'

# Test Docker installation
docker run hello-world
```



#### Dockerfile
```bash
# Use the latest Ubuntu 22.04 LTS as the base image
FROM ubuntu:22.04

# Update the package repository and install the required packages
RUN apt-get update && \
    apt-get install -y \
        apache2 \
        openssh-server \
        && \
    rm -rf /var/lib/apt/lists/*

# Create a new user called "student"
RUN useradd -m docker-user && \
    echo "docker-user:password" | chpasswd

# Give the htb-student user full access to the Apache and SSH services
RUN chown -R docker-user:docker-user /var/www/html && \
    chown -R docker-user:docker-user /var/run/apache2 && \
    chown -R docker-user:docker-user /var/log/apache2 && \
    chown -R docker-user:docker-user /var/lock/apache2 && \
    usermod -aG sudo docker-user && \
    echo "docker-user ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Expose the required ports
EXPOSE 22 80

# Start the SSH and Apache services
CMD service ssh start && /usr/sbin/apache2ctl -D FOREGROUND
```

#### Docker Build

```
docker build -t FS_docker .
```


```
docker run -p <host port>:<docker port> -d <docker container name>
```


```
docker run -p 8022:22 -p 8080:80 -d FS_docker
```
#docker

```
sudo apt-get install lxc lxc-utils -y
```
#lxc

```
sudo lxc-create -n linuxcontainer -t ubuntu
```
#lxc 



```
sudo systemctl restart networking
```


## VNC

```
sudo apt install xfce4 xfce4-goodies tigervnc-standalone-server -y
```

```
vncpasswd
```

```
touch ~/.vnc/xstartup ~/.vnc/config
cat <<EOT >> ~/.vnc/xstartup
```

```
cat <<EOT >> ~/.vnc/config
```

```
vncserver
```

```
vncserver -list
```

```
ssh -L 5901:127.0.0.1:5901 -N -f -l htb-student 10.129.14.130
```

```
xtightvncviewer localhost:5901
```
#vnc #ssh #xtightvncviewer



## Hosting a Rogue LDAP Server

```
sudo apt-get update && sudo apt-get -y install slapd ldap-utils && sudo systemctl enable slapd
```
#openldap




| `find / -group GROUPNAME 2>/dev/null` | Retrieve a list of files and directories owned by a specific group.       |
| ------------------------------------- | ------------------------------------------------------------------------- |
| `find / -perm -o+w 2>/dev/null`       | Retrieve a list of all world-writable files and directories.              |
| `find / -type f -cmin -5 2>/dev/null` | Retrieve a list of files created or changed within the last five minutes. |

``
```
stat /etc/hosts
```
#stat



Identifying Groups

In Linux systems, certain groups grant specific privileges that attackers may target to escalate their privileges. Some important Linux groups that might be of interest to an attacker include:

- **sudo** or **wheel**: Members of the sudo (or wheel) group have the authority to execute commands with elevated privileges using sudo.
- **adm**: The adm group typically has read access to system log files.
- **shadow**: The shadow group is related to managing user authentication and password information. With this membership, a user can read the `/etc/shadow` file, which contains the password hashes of all users on the system.
- **disk**: Members of the disk group have almost unrestricted read and limited write access inside the system.

```
getent group adm
```
#getent 



```
find / -perm -u=s -type f 2>/dev/null

```

```
/usr/bin/python3.8 -c 'import os; os.execl("/bin/sh", "sh", "-p", "-c", "cp /bin/bash /var/tmp
/bash && chown root:root /var/tmp/bash && chmod +s /var/tmp/bash")'
```


```
sudo debsums -e -s
```

```
strings example.elf
```

```
find / -perm -u=s -type f 2>/dev/null
```


		 one liner to search for all crontabs user level
```
sudo bash -c 'for user in $(cut -f1 -d: /etc/passwd); do entries=$(crontab -u $user -l 2>/dev/null | grep -v "^#"); if [ -n "$entries" ]; then echo "$user: Crontab entry found!"; echo "$entries"; echo; fi; done'
```
#crontab 


Enumerating User Autostart Scripts

```
ls -a /home/*/.config/autostart
```

Listing All System Services

```
sudo systemctl list-units --all --type=service
```

Listing Running Services

```
sudo systemctl list-units --type=service --state=running
```

Viewing the Backdoor Service

```
 sudo systemctl status b4ckd00rftw.service
```


Finding Browser Artefact Directories

```c
sudo find /home -type d \( -path "*/.mozilla/firefox" -o -path "*/.config/google-chrome" \) 2>/dev/null
```
