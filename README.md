# Zitadel-with-Postgres

Installing the Latest PostgreSQL (current 16.2.1) and configurations needed.

## Prerequisite:

 * Ubuntu 22.0.4   4 CPU and 4 GB RAM
 * CockroachDB Version 23.x.x completed
 * Nginx setup and working.
 * apt update && apt upgrade
 * timedatectl set-timezone America/Chicago
 * apt install net-tools
 * apt install mlocate
 * apt install make
 * network configurations (static IP Address & DNS)
 * Certificates are completed.

Update System Packages

```
apt update && sudo apt upgrade -y
```

## Repository  

```
sh -c 'echo "deb https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```

## Import the repository signing key:

```
curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
```

## Update package list

```
apt  update
```

## Install

```
apt -y install postgresql
```

## Check Status 

```
systemctl status postgresql
```

## Login

```
sudo -u postgres psql
```

## Database setup 

```
CREATE ROLE zitadel LOGIN;
CREATE DATABASE zitadel;
GRANT CONNECT, CREATE ON DATABASE zitadel TO zitadel;
```

## Zitadel Database configuration

```
Database:
  postgres:
    Host: localhost
    Port: 5432
    Database: zitadel
    MaxOpenConns: 25
    MaxIdleConns: 10
    MaxConnLifetime: 1h
    MaxConnIdleTime: 5m
    Options:
    User:
      Username: zitadel
      Password: zitadel
      SSL:
        Mode: disable
        RootCert:
        Cert:
        Key:
    Admin:
      Username: postgres
      Password: postgres
      SSL:
        Mode: disable
        RootCert:
        Cert:
        Key:
```
## Test connection

```
psql --username zitadel --password --host localhost zitadel
```

## Start-Init Zitadel Instance

```
zitadel start-from-init   --config defaults.yaml  --masterkey "MasterkeyNeedsToHave32Characters"  --tlsMode external
```

#  Postgres Database commands

## change database
```
\c zitadel
```
## Show all tables 
```
\dt+ *.* (shows all information on each table)
\dt projections.* (show all table in Projections schema)
\dt projections.users8_humans ( shows just the schema and table 'users8_humans')
```
## show data in all collumns
```
SELECT * FROM projections.users8_humans;
```




