---
description: clis
---

# Linux Terminal Commands

## Grep

search for strings in current directory recusive also for sub directories

```
grep -r "your_string" .
```

show only filenames:

```
grep -rl "your_string" .
```

ignore dir like node\_modules:

```
grep -r --exclude-dir=node_modules "your_string" .
```



## find and replace

```
sed 's/foo/bar/g'
```

## Find

search files:

```
find . -type f -name "name*"
```



search directories:

```
find . -type d -name "name*"
```

```
find . -maxdepth 2 -type d -name "name*"
```

```
find . -type d -name "name*" | sort
```



## Ports

```
ss -tulp
```

```
lsof -i :8080
```





## Disk

### Disk usage

```
df -h
```

```
du -ah . | sort -rh | head -20
```



### Disk partitions

```
lsblk
```

```
cfdisk
```

### Clean disk

```
ncdu
```



## Processes

```
htop
```

```
btop
```



## HTTP Requests

```
curl http://localhost:8080
```



### File Transfer / Sync

* **rsync**
* **scp**
* **ssh**

## Text editor

vim&#x20;

nvim

## Select options

fzf



## Container

podman

docker&#x20;

kubectl





## Logs

journalctl -u nginx



## Systemd





#### Service control

```bash
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
systemctl reload nginx
systemctl status nginx
```

#### Enable at boot

```bash
systemctl enable nginx
systemctl disable nginx
systemctl enable --now nginx
```

#### Listing

```bash
systemctl list-units --type=service
systemctl --failed
```

#### System actions

```bash
systemctl reboot
systemctl poweroff
systemctl suspend
```

## Service Unit Files

Typical locations:

```
/etc/systemd/system/
/usr/lib/systemd/system/
/lib/systemd/system/
```

Example service file:

```
[Unit]
Description=My App

[Service]
ExecStart=/usr/bin/myapp
Restart=always

[Install]
WantedBy=multi-user.target
```

## User Services

User-level services:

```
systemctl --user status
systemctl --user start myservice
```

#### Reload units

```bash
systemctl daemon-reload
```

#### Boot analysis

```bash
systemd-analyze
systemd-analyze blame
```

## Mask / Unmask Services

Prevent service from starting:

```
systemctl mask nginx
systemctl unmask nginx
```

#### Only running

```
systemctl list-units --type=service --state=running
```

#### Failed services

```
systemctl --failed
```

## Service Information

#### Show unit details

```
systemctl show nginx
```

#### Show unit file

```
systemctl cat nginx
```

#### Show dependencies

```
systemctl list-dependencies nginx
```

## System Power Commands

```
systemctl reboot
systemctl poweroff
systemctl suspend
systemctl hibernate
```

***



