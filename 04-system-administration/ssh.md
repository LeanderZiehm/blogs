# SSH Server

#### Install SSH Server

```
sudo apt install ssh-server
systemctl enable ssh
```

#### Setup Key Access

```
scp ~/.ssh/public-key.pub user@100.100.100.100:~
mkdir .ssh/
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

#### Harden your sshd config

(optional: backup your config)

```
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
```

Search for the line that contains PasswordAuthentication and change it from yes to no and uncomment the line by removing the beginning #

you can edit sshd\_config with your favorite text editor:

<pre data-full-width="false"><code><strong>sudo vim /etc/ssh/sshd_config
</strong></code></pre>

or

you can automate the process by using the sed command:

```
sudo sed -i 's/^#?PasswordAuthentication .*/PasswordAuthentication yes/' /etc/ssh/sshd_config
```

then restart ssh demon with

```
sudo systemctl restart sshd
```



























## SSH Tunneling User

## 1. Create a dedicated user

```
sudo adduser ssh_port_tunneling_user
```

Set a password (you can also disable password auth later and use keys only).

```
openssl rand -base64 64
```

***

## 3. Create SSH directory

```
sudo mkdir -p /home/ssh_port_tunneling_user/.ssh
sudo chmod 700 /home/ssh_port_tunneling_user/.ssh
sudo chown ssh_port_tunneling_user:ssh_port_tunneling_user /home/ssh_port_tunneling_user/.ssh
```

***

## 4. Add SSH public key (recommended)

On your client machine:

```
ssh-keygen -t ed25519
```

add key to:

```
sudo vim /home/ssh_port_tunneling_user/.ssh/authorized_keys
```

Then fix permissions:

```
sudo chmod 600 /home/ssh_port_tunneling_user/.ssh/authorized_keys
sudo chown ssh_port_tunneling_user:ssh_port_tunneling_user /home/ssh_port_tunneling_user/.ssh/authorized_keys
```

***

## 5. Restrict SSH to port forwarding only

Edit SSH config:

```
sudo vim /etc/ssh/sshd_config
```

Add at the bottom:

```
Match User ssh_port_tunneling_user    AllowTcpForwarding yes    X11Forwarding no    PermitTTY no    ForceCommand /bin/false
```

#### Important meaning:

* `AllowTcpForwarding yes` → allows tunnels
* `PermitTTY no` → no interactive session
* `ForceCommand /bin/false` → blocks shell completely
* only SSH tunnel traffic works

***

## 6. (Stronger) restrict which ports can be forwarded

If you want tighter security:

```
Match User ssh_port_tunneling_user    AllowTcpForwarding yes    PermitOpen 127.0.0.1:9200 127.0.0.1:5601    PermitTTY no    ForceCommand /bin/false
```

This ensures the user can ONLY tunnel to Elasticsearch/Kibana.

***

## 7. Restart SSH

```
sudo systemctl restart ssh
```

***

## 8. Test SSH tunnel from a remote machine

#### Tunnel Elasticsearch:

```
ssh -i ~/.ssh/id_ed25519 \-L 9200:localhost:9200 \es_tunnel@your-server-ip
```

Now locally on your client:

```
curl http://localhost:9200
```

***

#### Tunnel Kibana:

```
ssh -i ~/.ssh/id_ed25519 \-L 5601:localhost:5601 \es_tunnel@your-server-ip
```

Then open:

```
http://localhost:5601
```

<br>

***







## SSH Client



## Generate Key

run:

```
ssh-keygen 
```

secure algorithm:

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

change passphrase:

```shell
ssh-keygen -p -f ~/.ssh/id_ed25519
```

## SSH Config

vim \~/.ssh/config

```
Host shortname
    HostName 100.100.100.100
    User ubuntu
    IdentityFile ~/.ssh/private-key-filename.key
```

## SSH Agent

save you time with passphrase automation.

check if your ssh agent is running:

```
eval "$(ssh-agent -s)"
```

add ssh key:

```
ssh-add ~/.ssh/id_ed25519
```

or set timeout period

```
ssh-add -t 8h ~/.ssh/id_ed25519
```

check loaded keys:

```
ssh-add -l
```
