# Linux Encryption Secrets Passwords

## Generate password:

```
tr -dc 'A-Za-z0-9' < /dev/urandom | head -c 64; echo
```

Others:\
openssl rand -base64 18\
python3 -c "import secrets; print(secrets.token\_urlsafe(24))"\
​\
​

## Topics&#x20;

gpg vs openssl\
GPG (GNU Privacy Guard)OpenSSL

KeePassXC

```
sudo apt update && sudo apt install keepassxc
```

<br>
