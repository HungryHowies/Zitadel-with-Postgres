# Zitadel-with-Postgres

Installing the Latest PostgreSQL (current 16.2.1) and configurations needed. This Documentation  is a basic demo on  using Postgres with Zitadel Self-hosting.

## Prerequisite:

 * Ubuntu 22.0.4   4 CPU and 4 GB RAM 
 * Nginx setup and working. 
 * timedatectl set-timezone America/Chicago 
 * network configurations (static IP Address & DNS)
 * Certificates are completed.

## Update System Packages

```
apt update &&  apt upgrade -y
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
## Check Verion

```
psql --version
```
```
root@zitadel:/home/ubuntu# psql --version
psql (PostgreSQL) 16.2 (Ubuntu 16.2-1.pgdg22.04+1)
root@zitadel:/home/ubuntu#
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
ALTER USER zitadel PASSWORD 'zitadel';
ALTER USER postgres PASSWORD 'postgres';
```

edit file pg_hba.conf and add statement

```
local   all             postgres                                   trust
```
## Zitadel Database configuration

This is part of the configurtins need the rest is not shown.

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
zitadel start-from-init   --config defaults.yaml --steps steps.yaml --masterkey "MasterkeyNeedsToHave32Characters"  --tlsMode external
```

#  Postgres Database commands

## Shell
```
sudo -u postgres psql
```

## Change database

```
\c zitadel
```
## Show all tables 
```
\dt+ *.* (shows all information on each table)
\dt projections.* (show all table in Projections schema)
\dt projections.users8_humans ( shows just the schema and table 'users8_humans')
```
## Show data in all collumns
```
SELECT * FROM projections.users8_humans;
```




