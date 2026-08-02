# Postgesql

## Install

```
sudo apt install postgresql
```

```
sudo dnf install postgresql
```

## A: Postgres On Bare Metal

```
# Install the server packages
sudo dnf install postgresql-server postgresql-contrib
# Initialize the database cluster
sudo postgresql-setup --initdb
```

## Enable postgres

```
sudo systemctl start postgresql
sudo systemctl enable postgresql
systemctl status postgresql
```

```
sudo -iu postgres psql
```



## B: Postgres on Podman:

```
podman run --name pg18 -e POSTGRES_PASSWORD=postgres -e POSTGRES_USER=postgres -e POSTGRES_DB=postgres -p 5432:5432 -v pg18data:/var/lib/postgresql -d docker.io/library/postgres:18
```

```
psql -h localhost -p 5432 -U postgres -d postgres
```



## New user and db

sudo -iu postgres createuser --interactive\
sudo -iu postgres createdb mydb\
psql -U myuser -d mydb

```
CREATE ROLE myapp_user
ALTER USER myapp_user WITH PASSWORD 'new_password';

CREATE DATABASE myapp_db
OWNER myapp_user;
```

## Change passowrd

```
sudo -i -u postgres psql
```

```
ALTER USER postgres WITH PASSWORD 'new_password';
\q
```

```
sudo systemctl restart postgresql
```

```
sudo -iu postgres psql -c "ALTER USER postgres WITH PASSWORD 'newpassword';"
```

## Connect

```
REVOKE CONNECT ON DATABASE analytics FROM PUBLIC;
GRANT CONNECT ON DATABASE analytics TO analytics_user;

REVOKE CONNECT ON DATABASE odoo FROM PUBLIC;
GRANT CONNECT ON DATABASE odoo TO odoo_user;
```

## Expose Postgres Publicly

{% hint style="danger" %}
This action is dangerous. Only follow if you follow have strong security practices and you know what you are doing.
{% endhint %}

```
sudo vim /etc/postgresql/16/main/postgresql.conf
```

listen\_addresses = '\*'

```
sudo vim /etc/postgresql/16/main/pg_hba.conf
```

host all all 0.0.0.0/0 md5

```
sudo systemctl restart postgresql
```

and also open firewall at 5432:

{% content-ref url="firewall.md" %}
[firewall.md](firewall.md)
{% endcontent-ref %}

## Backup:

```
pg_dumpall -U postgres -f all_databases_backup.sql
```

Copy to local storage

```
scp ./oracle-2_all_databases_backup.sql oracle:~/
"or"
rsync -avz -P oracle:/var/lib/postgresql/oracle-2_all_databases_backup.sql ~/
```

or archive in s3 compatible storage

## Restore Backup

if you have a backup on the personal computer and want it on the server:

```
scp ./pg_dumpall_2026-02-25_19-26-40.sql.gz database:~/
```

on server then load the database by:

```
gunzip ./pg_dumpall_2026-02-25_19-26-40.sql.gz
sudo chown postgres:postgres pg_dumpall_2026-02-25_19-26-40.sql
```

```
sudo mv /home/ubuntu/pg_dumpall_2026-02-25_19-26-40.sql /var/lib/postgresql/
```

```
sudo -iu postgres
```

```
psql -f ./pg_dumpall_2026-02-25_19-26-40.sql
```

## Backup with Rclone to cloud

```
sudo apt install rclone
```

on server

```
rclone config 
```

name it dropbox and select it (13)

on personal computer:

```
rclone authorize "dropbox"
```

copy paste auth key

test:

```
rclone copy /home/ubuntu/test-backup dropbox:backups --ignore-existing --stats=1m --log-level NOTICE
```

copy file:

```
https://raw.githubusercontent.com/LeanderZiehm/devops/refs/heads/main/backup-postgres.sh
```

```
chmod +x ./backup-postgres.sh
```

test:

```
crontab -e

* * * * * ~/backup-postgres.sh >> ~/pg_backup.log 2>&1
```

## Paths

config:

```
/etc/postgres
```

data:

```
/var/lib/postgres
```

todo:

migrations, expand contract.

## Securety and Isolation

```

WITH
db_privs AS (
    SELECT
        'DATABASE' AS object_type,
        datname AS object_name,
        NULL::text AS schema_name,
        NULL::text AS detail,
        has_database_privilege(current_user, datname, 'CONNECT') AS can_connect,
        has_database_privilege(current_user, datname, 'CREATE') AS can_create,
        has_database_privilege(current_user, datname, 'TEMP') AS can_temp
    FROM pg_database
),

role_privs AS (
    SELECT
        'ROLE' AS object_type,
        rolname AS object_name,
        NULL::text AS schema_name,
        (
            'superuser=' || rolsuper ||
            ', createdb=' || rolcreatedb ||
            ', createrole=' || rolcreaterole ||
            ', bypassrls=' || rolbypassrls ||
            ', login=' || rolcanlogin
        ) AS detail,
        NULL::boolean AS can_connect,
        NULL::boolean AS can_create,
        NULL::boolean AS can_temp
    FROM pg_roles
    WHERE rolname = current_user
),

schema_privs AS (
    SELECT
        'SCHEMA' AS object_type,
        nspname AS object_name,
        nspname AS schema_name,
        NULL::text AS detail,
        has_schema_privilege(current_user, nspname, 'USAGE') AS can_connect,
        has_schema_privilege(current_user, nspname, 'CREATE') AS can_create,
        NULL::boolean AS can_temp
    FROM pg_namespace
    WHERE nspname NOT IN ('pg_catalog', 'information_schema')
),



func_privs AS (
    SELECT
        'FUNCTION_SECURITY_DEFINER' AS object_type,
        p.proname AS object_name,
        n.nspname AS schema_name,
        'security_definer=true' AS detail,
        NULL::boolean AS can_connect,
        NULL::boolean AS can_create,
        NULL::boolean AS can_temp
    FROM pg_proc p
    JOIN pg_namespace n ON n.oid = p.pronamespace
    WHERE p.prosecdef = true
),

ext_privs AS (
    SELECT
        'EXTENSION' AS object_type,
        extname AS object_name,
        extnamespace::regnamespace::text AS schema_name,
        'owned_by=' || extowner::regrole::text AS detail,
        NULL::boolean,
        NULL::boolean,
        NULL::boolean
    FROM pg_extension
)

SELECT *
FROM db_privs

UNION ALL
SELECT * FROM role_privs
UNION ALL
SELECT * FROM schema_privs
UNION ALL
SELECT * FROM func_privs
UNION ALL
SELECT * FROM ext_privs

ORDER BY object_type, schema_name NULLS FIRST, object_name;
```

```
SELECT
  current_user,
  (SELECT count(*) FROM pg_database) AS db_count,
  (SELECT count(*) FROM pg_roles) AS role_count,
  (SELECT count(*) FROM pg_namespace) AS schema_count;
```

```
SELECT datname,
       has_database_privilege(current_user, datname, 'CONNECT') AS can_connect,
       has_database_privilege(current_user, datname, 'CREATE') AS can_create,
       has_database_privilege(current_user, datname, 'TEMP') AS can_temp
FROM pg_database
ORDER BY datname;
```

```
SELECT rolname,
       rolsuper,
       rolcreatedb,
       rolcreaterole,
       rolreplication,
       rolbypassrls,
       rolcanlogin
FROM pg_roles
ORDER BY rolname;
```

```
REVOKE CONNECT ON DATABASE postgres FROM PUBLIC;
```

```
GRANT CONNECT ON DATABASE odoo TO odoo_user;
```

***

## psql

```
psql -h domain.example.com -U postgresUser -d postgresDatabase
```

```
 psql -U postgresUser -h  domain.example.com  -d postgresDatabase -c "\copy public.texts(timestamp, text) FROM 'texts.csv' WITH (FORMAT csv)"
```

| Meta-Command               | Effect                                   |
| -------------------------- | ---------------------------------------- |
| \list or \l                | list all databases                       |
| \dt                        | Lists all tables in the current database |
| \du                        | show users                               |
| \conninfo                  | connection info                          |
| \change or \c databaseName | change database                          |
| \d tableName               | show schema of table                     |
| \d+ tableName              | detailed table infos                     |
|                            |                                          |

| **`\di`** | Lists all indexes.   | Helps with performance tuning and understanding database schema.          |
| --------- | -------------------- | ------------------------------------------------------------------------- |
| **`\dv`** | Lists all views.     | Essential for understanding virtual or pre-packaged data representations. |
| **`\df`** | Lists all functions. | Crucial for projects that rely heavily on stored procedures.              |
|           |                      |                                                                           |
|           |                      |                                                                           |

[https://www.postgresql.org/docs/current/app-psql.html](https://www.postgresql.org/docs/current/app-psql.html)

## Other

sudo -iu postgres initdb --locale=C.UTF-8 --encoding=UTF8 -D /var/lib/postgres/data

/etc/postgresql/16/main/postgresql.conf

## Related Topics

High availabiltiy

#### Patroni

[https://www.youtube.com/watch?v=cEWBC3a33ds](https://www.youtube.com/watch?v=cEWBC3a33ds)

[https://www.youtube.com/watch?v=RHwglGf\_z40](https://www.youtube.com/watch?v=RHwglGf_z40)

***
