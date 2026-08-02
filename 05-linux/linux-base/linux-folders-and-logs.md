# Linux Folders & Logs



`/var/log` # System log files



## Folder Structure



```
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```

| Directory        | Purpose                                                      |
| ---------------- | ------------------------------------------------------------ |
| `/`              | Root of the entire filesystem                                |
| `/bin`           | Essential user commands (e.g., `ls`, `cp`, `cat`)            |
| `/sbin`          | System administration commands                               |
| `/boot`          | Bootloader files and Linux kernel                            |
| `/dev`           | Device files (disks, terminals, USB devices)                 |
| `/etc`           | System-wide configuration files                              |
| `/home`          | User home directories (`/home/alice`)                        |
| `/root`          | Home directory of the root user                              |
| `/lib`, `/lib64` | Shared libraries needed by programs                          |
| `/media`         | Mount points for removable media (USB drives, CDs)           |
| `/mnt`           | Temporary mount points                                       |
| `/opt`           | Optional third-party software                                |
| `/proc`          | Virtual filesystem containing process and kernel information |
| `/run`           | Runtime data for processes                                   |
| `/srv`           | Data served by system services (web, FTP, etc.)              |
| `/sys`           | Kernel and hardware information                              |
| `/tmp`           | Temporary files                                              |
| `/usr`           | User programs, libraries, documentation                      |
| `/var`           | Variable data such as logs, caches, mail, databases          |

`/etc/group`  stores all groups and ids of linux and which users are part them\
`/etc/sudoers` stores groups that have sudo privilages like `/etc/sudoers.d/` &#x20;

`/etc/passwd` # User account information\
`/var/log` # System log files\
`/usr/bin` # Most executable programs<br>
