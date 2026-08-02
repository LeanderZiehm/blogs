# Linux Stats Info

On Ubuntu/Debian systems, the unified command is:

```bash
run-parts /etc/update-motd.d/
```

That is the closest equivalent to "show me the login message".

You can also use:

```bash
cat /run/motd.dynamic
```

On many Ubuntu systems, `/run/motd.dynamic` contains the generated MOTD from `update-motd`.

The login flow is roughly:

```
ssh database
      |
      v
sshd (accepts connection)
      |
      v
login shell starts
      |
      v
pam_motd runs
      |
      v
/etc/update-motd.d/* scripts execute
      |
      v
MOTD displayed
```

The component responsible is PAM:

```bash
grep motd /etc/pam.d/sshd
```

You will usually see something like:

```
session optional pam_motd.so motd=/run/motd.dynamic
session optional pam_motd.so noupdate
```

So:

* **SSH?** → transports the session only.
* **PAM (`pam_motd`)** → triggers the message display.
* **`update-motd` scripts** → generate the content.
* **Ubuntu packages** (landscape, update-notifier, etc.) → provide many of the individual sections.

If you want the exact same thing an SSH login triggers, you can test with:

```bash
sudo -u "$USER" /usr/bin/env bash -lc 'cat /run/motd.dynamic'
```

but `run-parts /etc/update-motd.d/` is the usual admin command.
