# Podman

I liked docker but when researching more about securety the systematic design of podman to be rootless by default thereby making it more secure against exploits won me over.

### Install

```
sudo apt install podman
```

## Postgres

```
podman run --name pg18 -e POSTGRES_PASSWORD=postgres -e POSTGRES_USER=postgres -e POSTGRES_DB=postgres -p 5432:5432 -v pg18data:/var/lib/postgresql -d docker.io/library/postgres:18
```



## Commands

example oneliners:

```
podman images
```

```
podman system df
```

```
podman run -it --entrypoint sh localhost/api-kv:latest
```

```
podman kill $(podman ps -q)
```

```
podman run -it docker.io/library/debian:13 /bin/bash
```

```
podman run -it --systemd=always --privileged docker.io/library/debian:13 /bin/bash
```

```
podman inspect wordpress-stack_db_1 | grep -i MYSQL
```

```
podman ps -a
```

```
podman exec -it <container_id_or_name> /bin/bash
```

```
podman run --name pg16 \
  -e POSTGRES_PASSWORD=postgres \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_DB=postgres \
  -p 5432:5432 \
  -v pgdata:/var/lib/postgresql/data \
  -d docker.io/library/postgres:16
```

```
podman volume ls
```

```
podman cp full_recovery_20260530_160846.sql pg16:/tmp/dump.sql
```

```
podman exec -it pg16 psql -U postgres -f /tmp/dump.sql
```

```
podman logs pg16
```

```
podman exec -it pg16 bash
```

```
podman exec -e MYSQL_PWD=password-here wordpress-stack_db_1 mysqldump -u user-here --single-transaction --routines --triggers --no-tablespaces databse-here > wordpress_backup.sql
```

***

```
podman pull docker.io/kalilinux/kali-rolling
podman inspect --format='{{index .RepoDigests 0}}' kalilinux/kali-rolling
```

## Podman Compose Examples:

```
sudo apt install podman-compose
```

todo

## Setup docker repos

if you want smoother docker compatebility change the config to default to docker repos:

```
sudo vim /etc/containers/registries.conf
```

```
unqualified-search-registries = ["docker.io"]
```
