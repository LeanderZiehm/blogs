# Linux Systemd & Journal

```
systemd-analyze
```



The command-line tools that come with systemd form a fairly large ecosystem. They cover service control, logging, system inspection, hardware, networking, and system utilities.

Here is a structured overview of the main systemd CLI tools:

***

#### Core service & system control

* **systemctl** – start/stop/enable/disable services, manage system state
* **systemd** (PID 1) – the init system itself (not usually invoked directly)
* **systemd-run** – run transient units from the command line
* **systemd-inhibit** – block shutdown/sleep while a process runs
* **systemd-ask-password** / **systemd-tty-ask-password-agent** – password prompting utilities

***

#### Logging & diagnostics

* **journalctl** – query and display logs from the journal
* **systemd-analyze** – boot time analysis, dependency graphs, critical chain
* **systemd-cgls** – view control group tree
* **systemd-cgtop** – real-time cgroup resource usage

***

#### System information tools

* **hostnamectl** – set/view hostname and machine metadata
* **timedatectl** – timezone and clock configuration
* **localectl** – locale and keyboard layout settings
* **loginctl** – user sessions, seats, and logins
* **resolvectl** – DNS resolver configuration (via systemd-resolved)

***

#### Unit / system management helpers

* **systemd-escape** – escape strings for unit names
* **systemd-tty-ask-password-agent** – password agent for services
* **systemd-detect-virt** – detect virtualization environment
* **systemd-sysusers** – manage system users from config files
* **systemd-tmpfiles** – manage temporary files and directories
* **systemd-sysctl** – apply kernel parameter settings

***

#### Device, boot & hardware related

* **systemd-hwdb** – hardware database tool
* **systemd-udevd** (daemon) + **udevadm** – device management interface
* **systemd-boot** tools (like `bootctl`) – bootloader management

***

#### Networking & runtime utilities

* **systemd-networkd** (daemon) + **networkctl** – network configuration and status
* **systemd-resolve** (legacy alias for resolvectl in some systems)
* **systemd-socket-activate** – socket activation wrapper

***

#### Miscellaneous / advanced

* **systemd-path** – show system and user directory paths
* **systemd-notify** – send service status updates to systemd
* **systemd-stdio-bridge** – bridge stdio to systemd socket activation
* **systemd-firstboot** – initial system setup tool
* **systemd-sysext** – manage system extension images

***

daily use:

* `systemctl`
* `journalctl`
* `loginctl`
* `hostnamectl`
* `timedatectl`





***



#### 1. Everyday admin tools&#x20;

These are the most important ones:

* systemctl – control services and units
* journalctl – read logs
* loginctl – user sessions
* hostnamectl – hostname/system identity
* timedatectl – time & timezone
* networkctl – network status (if using systemd-networkd)

***

#### 2. System inspection & debugging tools

Used for troubleshooting:

* systemd-analyze (boot performance, dependency graphs)
* systemd-cgls / systemd-cgtop (resource usage via cgroups)
* systemd-detect-virt (check virtualization)
* systemd-escape (unit name debugging helper)
* coredumpctl (crash dump inspection)

***

#### 3. System configuration utilities

* localectl (keyboard/locale)
* resolvectl (DNS via systemd-resolved)
* systemd-sysctl (kernel parameters)
* systemd-tmpfiles (temporary file rules)
* systemd-sysusers (user/group provisioning)

***

#### 4. Boot, hardware, and low-level system

* bootctl (EFI bootloader control)
* udevadm / systemd-udevd (device management)
* systemd-hwdb (hardware database)
* kernel-install (kernel management)
* systemd-repart (partition management)

***

#### 5. Security / advanced infrastructure

More modern / enterprise features:

* systemd-cryptsetup / systemd-cryptenroll (disk encryption)
* systemd-creds (secure credentials handling)
* systemd-measure (TPM2 / measured boot)
* systemd-sysext (system extensions)
* systemd-nspawn / vmspawn (containers / VMs)

***

#### 6. Networking stack (systemd-integrated)

If systemd manages networking:

* systemd-networkd (network daemon)
* systemd-resolved (DNS resolver)
* resolvectl (DNS querying tool)
* systemd-network-generator (auto network configs)

***

#### 7. System services exposed as CLI tools

These are less “commands” and more interfaces to daemons:

* systemd-logind (login/session manager)
* systemd-machined (machines/containers)
* systemd-oomd (out-of-memory daemon)
* systemd-journald tools (logging backend)



***

### 1) Basic journal viewing

* `journalctl` → show all logs (can be very long)
* `journalctl -b` → logs from current boot
* `journalctl -b -1` → previous boot logs
* `journalctl -f` → follow logs live (like `tail -f`)

***

### 2) Filtering logs

#### By service (very common)

* `journalctl -u ssh`
* `journalctl -u nginx`
* `journalctl -u systemd-networkd`

#### By time

* `journalctl --since "today"`
* `journalctl --since "1 hour ago"`
* `journalctl --since "2026-06-01 10:00:00"`
* `journalctl --until "2026-06-01 12:00:00"`

***

### 3) Priority / severity filtering

* `journalctl -p 0` → emergencies only
* `journalctl -p 3` → errors and above
* `journalctl -p warning`
* `journalctl -p info`

Priority levels:

* 0 = emergency
* 1 = alert
* 2 = critical
* 3 = error
* 4 = warning
* 5 = notice
* 6 = info
* 7 = debug

***

### 4) Kernel logs

* `journalctl -k` → kernel messages
* `journalctl -k -b` → kernel logs from current boot

***

### 5) Output formatting

* `journalctl -o short` → default readable format
* `journalctl -o verbose` → detailed fields
* `journalctl -o json` → machine-readable JSON

***

### 6) Searching logs

* `journalctl | grep ssh`
* `journalctl -u ssh | grep "Failed"`

Better option:

* `journalctl -u ssh --grep "failed"`

***

### 7) Disk usage & maintenance

* `journalctl --disk-usage` → size of logs
* `sudo journalctl --vacuum-time=7d` → keep only 7 days
* `sudo journalctl --vacuum-size=100M` → limit log size

***

#### Debug a service crash

```bash
journalctl -u nginx -b --no-pager
```

#### Watch live system errors

```bash
journalctl -p 3 -f
```

#### Check boot issues

```bash
journalctl -b -1 -p 3
```

***

`journalctl`

* `-u` = service-focused debugging
* `-b` = boot history analysis
* `-f` = real-time monitoring

