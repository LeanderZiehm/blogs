# Linux Users, Groups, Permissions

## Add user:



#### Untrusted User

```
sudo adduser llm
sudo passwd llm
```











#### Secret User

```
sudo useradd -m -s /bin/bash secret_holder
sudo passwd secret_holder
```

```
ls -ld /home/secret_holder
```



```bash
useradd -m -G wheel -s /bin/bash username
passwd username
```

## Give sudo permissions:

make sure you have sudo installed and if it doest work try:

```bash
EDITOR=vim visudo
```

add user to group:

```
sudo usermod -aG google-sudoers username
```

***



## Switch user

```
sudo -iu secret_holder
```

root

```
sudo -i
```

## Check Loggedin Users:

```
who
```

```
w
```









## Notify on Sudo use.



```
sudo apt install auditd 
```

```
sudo systemctl enable --now auditd
sudo systemctl status auditd
```



```
id llm
```



```
sudo vim /etc/audit/rules.d/llm-sudo.rules
```

```
-a always,exit -F arch=b64 -S execve -F auid=1002 -F path=/usr/bin/sudo -k llm-sudo
```

```
sudo augenrules --load
```









sudo useradd -m llmaicode\
sudo passwd llmaicode

su - llmaicode\
su - alice

https://www.debian.org/distrib/

authorized\_keys

i am on debian and i am just about to give someone i don't trust access to my computer with user privilges how to make it so that i can see all actions they do, accept or reject their sudo request per notification have a snapshot before they got access to role back and see diff what they added after snapshot for audit

i guess qemu vm with kvm i will do to have it more isolated and code as infra or setup reproducable and the stuff you say just give me all commands i need to do to install and setup and thne makefile and other setups

***

### 1) Users

#### See users

* `whoami` → shows current user
* `id` → shows user ID (UID), groups, and group IDs
* `cat /etc/passwd` → list all system users

#### Create / modify users (admin required)

* `sudo useradd username` → create user
* `sudo passwd username` → set password
* `sudo usermod -aG groupname username` → add user to group
* `sudo userdel username` → delete user

***

### 2) Groups

#### View groups

* `groups` → groups of current user
* `groups username` → groups of another user
* `cat /etc/group` → all groups

#### Manage groups

* `sudo groupadd groupname` → create group
* `sudo groupdel groupname` → delete group
* `sudo usermod -aG groupname username` → add user to group

***

### 3) Permissions (core concept)

Each file has:

* **Owner (user)**
* **Group**
* **Others**

Check permissions:

* `ls -l`

Example output:

```
-rwxr-xr--
```

Meaning:

* `r` = read
* `w` = write
* `x` = execute

Breakdown:

* `rwx` → owner
* `r-x` → group
* `r--` → others

***

### 4) Changing permissions

* `chmod 755 file`
* `chmod 644 file`

Meaning:

* 7 = rwx
* 6 = rw-
* 5 = r-x
* 4 = r--

***

### 5) Changing ownership

* `sudo chown user file`
* `sudo chown user:group file`
* `sudo chgrp group file`

***

### 6) extras

* `umask` → default permission mask
* `sudo` → run commands as admin
* `getent passwd` → query system user database cleanly

***

* Users = identity
* Groups = shared access buckets
* Permissions = what each bucket can do to files
