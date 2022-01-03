---
title: PostgreSQL HA setup
updated: 2015-10-09 10:38
---

## Synopsis
While there are many [posssibilities](https://wiki.postgresql.org/wiki/Replication,_Clustering,_and_Connection_Pooling) and options to cofigure a HA cluster for PostgreSQL. The most common real world implementation is based on [Patroni](https://github.com/zalando/patroni). Patroni requires a distributed configuration store, and there are several options to choose from, we will be using [etcd](https://github.com/etcd-io/etcd).

## Setup and Build
Download Hybris Commerce Suite 5602 from Hybris WIKI [https://download.hybris.com/resources/releases/5.6.0/hybris-commerce-suite-5.6.0.2.zip](https://download.hybris.com/resources/releases/5.6.0/hybris-commerce-suite-5.6.0.2.zip)
Unzip the file to your preferred local folder and execute following from command prompt.

``` bash
brew install postgresql
```

```bash
docker run -d \
    --name testpsql_node1 \
    -e POSTGRES_USER=postgres \
    -e POSTGRES_PASSWORD=postgres \
    -e POSTGRES_DB=postgres \
    -p 6551:5432 \
    -v testpsql_data:/var/lib/postgresql/data \
    postgres:latest

docker run -d \
    --name testpsql_node2 \
    -e POSTGRES_USER=postgres \
    -e POSTGRES_PASSWORD=postgres \
    -e POSTGRES_DB=postgres \
    -p 6552:5432 \
    -v testpsql_data:/var/lib/postgresql/data \
    postgres:latest
```

We can now test this by accessing the container.
```bash
docker exec -ti pg_node_1 bash
docker exec -ti pg_node_2 bash
```

Let's go ahead and test some of the basic admin commands to stop,start and restart the installed database
```bash
brew services stop postgresql
brew services start postgresql
brew services restart postgresql
```

Next, let us create a database user testpsql.
:point_right: PostgreSQL uses roles to represent user accounts, this is unlike other databases that use the user concept.

```bash
psql postgres
CREATE ROLE testpsql WITH LOGIN PASSWORD 'testpsql';
ALTER ROLE testpsql CREATEDB;
\q
```
You can always change the password by using this command.

```bash
ALTER ROLE testpsql WITH PASSWORD 'testpsql';
```
Next, we will login with the new user and create a database named testpsql and associate certain roles for the newly created user testpsql

```bash
psql postgres -U testpsql
CREATE DATABASE testpsql;
GRANT ALL PRIVILEGES ON DATABASE testpsql TO testpsql;
```

## Reference
* https://www.postgresql.org/docs/9.0/sql-createrole.html
* https://www.postgresql.org/docs/9.0/sql-alterrole.html
* https://cloud.google.com/architecture/architectures-high-availability-postgresql-clusters-compute-engine#postgressql_ha_architectures