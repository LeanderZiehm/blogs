# Linux Commands

```
pstree
```



```
ss -tulpn
```

* `-t` = TCP
* `-u` = UDP
* `-l` = listening ports
* `-p` = process
* `-n` = numeric (no DNS lookup)



```
last
```

```
who
w
```







check free storage:

```
df -h
```

check connected storage disks:

```
lsblk
```



mounting:

```
sudo mkdir -p /mnt/recovery
sudo mount /dev/sdb1 /mnt/recovery
```



```
sudo chown -R $USER:$USER main
```



## tar zip:

```
tar -czvf zipped_name.tar.gz file.name
```

* `-c` → create a new archive
* `-z` → compress it using **gzip**
* `-v` → verbose (shows progress)
* `-f` → output file name



```
tar -xzvf full_recovery_20260530_160846.tar.gz
```

* `-x` → extract
* `-z` → decompress it using **gzip**
* `-v` → verbose (shows progress)
* `-f` → file name

unpack tar:

```
tar -xzf postgresql_backup.tar.gz
```



```
sudo tar -czvf postgres_backup.tar.gz -C /mnt/recovery var/lib/postgresql
```







transfer folders and files:

```
rsync -aHAX --progress -e "ssh -i /path/to/private_key" /source/ user@host:/destination/
```

```
```



dmesg | tail -50

iscsiadm





## Common

ls -l

