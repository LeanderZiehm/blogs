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

(optional: backup your config)

```
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
```

Search for the line that contains PasswordAuthentication and change it from yes to no and uncomment the line by removing the beginning #

you can edit sshd\_config with your favorite text editor:

{% code fullWidth="false" %}
```
sudo vim /etc/ssh/sshd_config
```
{% endcode %}

or

you can automate the process by using the sed command:

```
sudo sed -i 's/^#?PasswordAuthentication .*/PasswordAuthentication yes/' /etc/ssh/sshd_config
```

then restart ssh demon with

```
sudo systemctl restart sshd
```



