# Linux Users

Add new user:

```bash
useradd -m -G wheel -s /bin/bash username
passwd username
```

Give sudo permissions:

make sure you have sudo installed and if it doest work try:

```bash
EDITOR=vim visudo
```

