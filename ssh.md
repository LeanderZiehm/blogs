---
description: guide
---

# SSH



## Install SSH Server

sudo apt install ssh-server

systemctl enable ssh



## Setup Key Access

scp \~/.ssh/public-key.pub user@100.100.100.100:\~

mkdir .ssh/\
touch \~/.ssh/authorized\_keys\
chmod 600 \~/.ssh/authorized\_keys\
chmod 700 \~/.ssh



## Harden your SSH config

backup your config:

`sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak`



Search for the line that contains PasswordAuthentication and change it from yes to no and uncomment the line by removing the beginning #

`sudo vim /etc/ssh/sshd_config`&#x20;

or

{% hint style="info" %}
sudo sed -i 's/^#?PasswordAuthentication .\*/PasswordAuthentication yes/' /etc/ssh/sshd\_config
{% endhint %}

then restart ssh demon with

`sudo systemctl restart sshd`



