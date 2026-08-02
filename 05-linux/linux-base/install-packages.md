# Linux Install Packages

## Flathub

```
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```



## Debian

update your system and packages:

```
sudo apt update && sudo apt upgrade -y
```

you can install the packages on debian by running:\
`sudo apt install put-packagename-here`

| Package               | Space Needed |
| --------------------- | ------------ |
| podman-compose        | 349 kB       |
| python3-pip           |              |
| arp-scan              | 17.8MB       |
| git-filter-repo       |              |
| mariadb-client-compat | 83.1 MB      |
| htop                  | 434 kB       |

| Package            | Approx. Size Installed | Dependencies (approx.) | Notes                    |
| ------------------ | ---------------------- | ---------------------- | ------------------------ |
| curl               | 12.6 MB                | 19+                    | CLI HTTP tool            |
| git                | 132 MB                 | 50+                    | Includes perl, ssh, etc. |
| python3            | 22.8 MB                | —                      | Base interpreter         |
| tmux               | 2 MB                   | —                      | Terminal multiplexer     |
| vim                | 45.9 MB                | 5                      | Includes runtime + xxd   |
| i3 (desktop)       | 429 MB                 | —                      | Window manager setup     |
| chromium (desktop) | 538 MB                 | —                      | Full browser             |
| xinit (desktop)    | 36.1 MB                | —                      | X session starter        |
| sudo               | 6.8 MB                 | —                      | Privilege escalation     |
| iputils-ping       | 2.8 MB                 | 3                      | Includes libidn2, etc.   |
| curl               | 21 MB                  |                        |                          |
|                    |                        |                        |                          |

| Package |
| ------- |
| mininet |
|         |
|         |

## RHEL
