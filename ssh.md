---
description: guide
---

# SSH



### Install SSH Server

```
sudo apt install ssh-server
systemctl enable ssh
```

### Setup Key Access

```
scp ~/.ssh/public-key.pub user@100.100.100.100:~
mkdir .ssh/
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

### Harden your SSH config

{% code title="(optional: backup your config)" %}
```
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
```
{% endcode %}

Search for the line that contains PasswordAuthentication and change it from yes to no and uncomment the line by removing the beginning #

{% code title="you can edit sshd_config with your favorite text editor" fullWidth="true" %}
```
sudo vim /etc/ssh/sshd_config
```
{% endcode %}

{% code title="or you can automate the process by using the sed command" %}
```
sudo sed -i 's/^#?PasswordAuthentication .*/PasswordAuthentication yes/' /etc/ssh/sshd_config
```
{% endcode %}

then restart ssh demon with

```
sudo systemctl restart sshd
```



