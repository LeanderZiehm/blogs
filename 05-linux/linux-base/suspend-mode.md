# Suspend Mode

check what your mode is&#x20;

```
cat /sys/power/mem_sleep
```

output: s2idle \[deep]

```
sudo vim /etc/default/grub
```

append mem\_sleep\_default=s2idle to GRUB\_CMDLINE\_LINUX\_DEFAULT&#x20;

GRUB\_CMDLINE\_LINUX\_DEFAULT="(some other options ...) mem\_sleep\_default=s2idle"



then update grup to apply changes

```
sudo update-grub
```

```
sudo reboot
```



confirm

```
cat /sys/power/mem_sleep
```



output: \[s2idle] \[deep]

