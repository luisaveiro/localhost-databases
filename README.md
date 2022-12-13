<p align="center">
    <a href="https://supportukrainenow.org" target="_blank">
        <img src="./images/standwithukraine.png" alt="#StandWithUkraine" width="100%">
    </a>
</p>
<br/>
<p align="center">
    <a href="http://github.com/luisaveiro/localhost-databases">
        <img 
            src="./images/databases.svg" 
            alt="databases" 
            width="50%">
    </a>
</p>

<h4 align="center">
    Collection of Database Docker compose files for local development
</h4>

<p align="center">
    <a href="#about">About</a> •
    <a href="#disclaimer">Disclaimer</a> •
    <a href="#getting-started">Getting Started</a> •
    <a href="#download">Download</a> •
    <a href="#how-to-use">How To Use</a> •
    <a href="#databases">Databases</a> •
    <a href="#docker-network">Docker Network</a>
</p>
<p align="center">
    <a href="#faq">FAQ</a> •
    <a href="#useful-tips">Useful Tips</a> •
    <a href="#changelog">Changelog</a> •
    <a href="#contributing">Contributing</a> •
    <a href="#security-vulnerabilities">Security Vulnerabilities</a> •
    <a href="#credits">Credits</a> •
    <a href="#Sponsor">Sponsor</a> •
    <a href="#license">License</a>
</p>

## About

This repository is a collection of Docker Compose files for relational and 
NoSQL databases. Which aims to offer a simple approach to setting up databases 
for a local environment.

**What is the purpose of the database collection?**  

As a developer, you might be working on multiple Docker-based projects. Your 
projects could be interacting with each other, e.g., a service mesh. Each 
service could have its database container with the same database engine.

Running all the containers locally on your computer could impact performance. 
You could experience the Docker container port binding failure message - 
*Bind for 0.0.0.0:3306 failed: port is already allocated.* Having a shared 
database container would resolve these issues.

Also this database collection allows you to learn and experiment with different 
database engines without you installing additional dependencies to use 
the databases.

## Disclaimer

**Please note:** ***Localhost Databases*** is not affiliated with the databases' 
developers/owners and is not an official product.

***Localhost Databases*** has been developed to run databases in a local 
Docker environment. To install a production instance, read the databases' 
respective installation guides.


## Getting Started

You will need to make sure your system meets the following prerequisites:

- Docker Engine >= 20.10.00

This repository utilizes [Docker](https://www.docker.com/) to run Databases, 
e.g., MySQL. So, before using ***Localhost Databases***, make sure you have 
Docker installed on your system.

## Download

You can clone the latest version of ***Localhost Databases*** repository for 
macOS, Linux and Windows.

```bash
# Clone this repository.
$ git clone git@github.com:luisaveiro/localhost-databases.git --branch main --single-branch
```

## How To Use

There are a few steps you need to follow before you can have a database set up 
and running in Docker container. I have outline the steps you would need to 
take to get started.

#### 1. <ins>Configuring your DotEnv file</ins>

Before you start a database in a Docker container, you will need to create a 
DotEnv file. The DotEnv file will allow you to configure your database's 
credentials and map a container's port.

***Localhost Databases*** includes a `.env.example` file. You can run the 
following command in the terminal to create your DotEnv file.

```bash
# Create .env from .env.example.
$ cp .env.example .env
```

Each database has its environment variables (below, I have provided more information). 
You have the option to modify each of the database's environment variables 
individually, or you can edit the referenced `Database` environment variables 
(prefixed with `DB_`).

```ini
#--------------------------------------------------------------------------
# Global Database env
#--------------------------------------------------------------------------

DB_DATABASE=local

DB_PORT=3306

DB_ROOT_PASSWORD=secret

DB_USERNAME=luisaveiro
DB_PASSWORD=password
```

#### 2. <ins>Start database container</ins>

After you configure your DotEnv, you can start a database container. Each 
database has its individual Docker Compose file. You will need to provide 
the database's Docker Compose file to the `docker compose` command by 
using the `-f` flag. 

```bash
docker compose -f compose.database-name.yaml up -d
```

An example of the `docker compose` command would be as follows:

```bash
docker compose -f compose.redis.yaml up -d
```

Docker will create the database container with the container name 
`local_dbs_redis` in our example. The container will be attached to a network 
called `local_dbs_network`.

If you want to change the container name or network name, you can edit the 
DotEnv file and override the Docker Compose variables. Below is an example of 
the DotEnv variables.

```ini
#--------------------------------------------------------------------------
# Docker env
#--------------------------------------------------------------------------

# Project name
APP_NAME="local_dbs"

# Docker containers network
NETWORK_NAME="${APP_NAME}_network"

#--------------------------------------------------------------------------
# Redis env
#--------------------------------------------------------------------------

REDIS_CONTAINER_NAME="${APP_NAME}_redis"
```

## Databases

Localhost Databases include 10 database servers. The following databases are 
part of this repository's collection:

- [Cassandra](#config-cassandra)
- [CockroachDB](#config-cockroachdb)
- [DynamoDB Local](#config-dynamodb)
- [MariaDB](#config-mariadb)
- [MongoDB](#config-mongodb)
- [Microsoft SQL Server (MSSQL)](#config-mssql)
- [MySQL](#config-mysql)
- [PostgreSQL](#config-postgres)
- [Redis](#config-redis)
- [SurrealDB](#config-surrealdb)
- [YugabyteDB](#config-yugabytedb)

Below I have provided more information on how to configure each database, 
start the database container and connect to the database via a database client 
app.

---

#### <a id="config-cassandra"></a> <ins>Configuring Cassandra</ins>

[Apache Cassandra](https://cassandra.apache.org) is an open source NoSQL 
distributed database that manages large amounts of data across commodity 
servers. It is a decentralized, scalable storage system designed to handle vast 
volumes of data across multiple commodity servers, providing high availability 
without a single point of failure.

##### **Environment Variables**

The Cassandra Docker Compose file uses the follow variables from the DotEnv 
file.

```ini
#--------------------------------------------------------------------------
# Cassandra env
#--------------------------------------------------------------------------

CASSANDRA_CONTAINER_NAME="${APP_NAME}_cassandra"

CASSANDRA_PORT=9042
```

**Please note:** You are unable to create additional users via the Cassandra 
Docker image environment variables.

##### **Start & Stop Docker container**

To start the Cassandra Local container, you can run the following command:

```bash
docker compose -f compose.cassandra.yaml up -d
```

To stop the Cassandra Local container, you can run the following command:

```bash
docker compose -f compose.cassandra.yaml down
```

##### **Connect to Database**

To connect to your Cassandra container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${CASSANDRA_PORT}"

USER="cassandra"
PASSWORD="cassandra"
```

Please note: The `cassandra` user is the system administrator account on the 
Cassandra Server instance that's created during setup.

Below is a screenshot of the settings used in TablePlus:

<p align="center">
    <a>
    <img 
        src="./images/tableplus-cassandra.png" 
        alt="TablePlus settings for Cassandra"
        width="50%">
    </a>
    <br>
    <sub><sup>TablePlus settings for Cassandra.</sup></sub>
</p>

---

#### <a id="config-cockroachdb"></a> <ins>Configuring CockroachDB</ins>

[CockroachDB](https://www.cockroachlabs.com/) is a distributed database with standard SQL for cloud applications.

##### **Environment Variables**

The CockroachDB Docker Compose file uses the follow variables from the DotEnv 
file.

```ini
#--------------------------------------------------------------------------
# CockroachDB env
#--------------------------------------------------------------------------

COCKROACHDB_CONTAINER_NAME="${APP_NAME}_cockroachdb"

COCKROACHDB_PORT=26257

COCKROACHDB_UI_PORT=8080
```

##### **Start & Stop Docker container**

To start the CockroachDB Local container, you can run the following command:

```bash
docker compose -f compose.cockroachdb.yaml up -d
```

To stop the CockroachDB Local container, you can run the following command:

```bash
docker compose -f compose.cockroachdb.yaml down
```

##### **Connect to Database**

To connect to your CockroachDB container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${COCKROACHDB_PORT}"

USER="root"
```

Please note: The CockroachDB root user does not have a password.

Below is a screenshot of the settings used in TablePlus:

<p align="center">
    <a>
    <img 
        src="./images/tableplus-cockroachdb.png" 
        alt="TablePlus settings for CockroachDB"
        width="50%">
    </a>
    <br>
    <sub><sup>TablePlus settings for CockroachDB.</sup></sub>
</p>

##### **Connect to DB Console**

The CockroachDB has a DB Console that gives you insight into the overall health 
of your cluster as well as the performance of the client workload. Go to 
http://localhost:8080.

Below is a screenshot of the DB Console:

<p align="center">
    <a>
    <img 
        src="./images/browser-cockroachdb.png" 
        alt="CockroachDB DB Console"
        width="50%">
    </a>
    <br>
    <sub><sup>CockroachDB DB Console.</sup></sub>
</p>

---

#### <a id="config-dynamodb"></a> <ins>Configuring DynamoDB Local</ins>

[Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html) 
is a fully managed NoSQL database service that provides fast and predictable 
performance with seamless scalability. DynamoDB local is a downloadable version 
of Amazon DynamoDB, you can develop and test applications without accessing the 
DynamoDB web service.

##### **Environment Variables**

The DynamoDB Local Docker Compose file uses the follow variables from the DotEnv 
file.

```ini
#--------------------------------------------------------------------------
# DynamoDB local env
#--------------------------------------------------------------------------

DYNAMODB_CONTAINER_NAME="${APP_NAME}_dynamodb"

DYNAMODB_PORT=8000
```

##### **Start & Stop Docker container**

To start the DynamoDB Local container, you can run the following command:

```bash
docker compose -f compose.dynamodb.yaml up -d
```

To stop the DynamoDB Local container, you can run the following command:

```bash
docker compose -f compose.dynamodb.yaml down
```

##### **Connect to Database**

> **Note**
> : TablePlus currently doesn't support DynamoDB. You can use NoSQL Workbench.

To connect to your DynamoDB Local container from your database client, you will 
need to provide the following settings:

```ini
HOST=localhost
PORT="${DYNAMODB_PORT}"
```

Below is a screenshot of the settings used in NoSQL Workbench:

<p align="center">
    <a>
    <img 
        src="./images/nosql-workbench-dynamodb.png" 
        alt="NoSQL Workbench settings for DynamoDB Local"
        width="50%">
    </a>
    <br>
    <sub><sup>NoSQL Workbench settings for DynamoDB Local.</sup></sub>
</p>

---

#### <a id="config-mariadb"></a> <ins>Configuring MariaDB</ins>

[MariaDB](https://mariadb.org/) Server is one of the most popular open source 
relational databases. It's made by the original developers of MySQL.

##### **Environment Variables**

The MariaDB Docker Compose file uses the follow variables from the DotEnv file.

```ini
#--------------------------------------------------------------------------
# MariaDB env
#--------------------------------------------------------------------------

MARIADB_CONTAINER_NAME="${APP_NAME}_mariadb"

MARIADB_DATABASE="${DB_DATABASE}"

MARIADB_PORT="${DB_PORT}"

MARIADB_ROOT_PASSWORD="${DB_ROOT_PASSWORD}"

MARIADB_USERNAME="${DB_USERNAME}"
MARIADB_PASSWORD="${DB_PASSWORD}"
```

**Please note:** MariaDB allows root's password to be empty.

For a list of available environment variables that the MariaDB Docker image 
supports, you can visit [MariaDB Docker Hub](https://hub.docker.com/_/mariadb) 
page.

##### **Start & Stop Docker container**

To start the MariaDB container, you can run the following command:

```bash
docker compose -f compose.mariadb.yaml up -d
```

To stop the MariaDB container, you can run the following command:

```bash
docker compose -f compose.mariadb.yaml down
```

##### **Connect to Database**

To connect to your MariaDB container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${MARIADB_PORT}"

USER="${MARIADB_USERNAME}"
PASSWORD="${MARIADB_PASSWORD}"
```

Below is a screenshot of the settings used in TablePlus:

<p align="center">
    <a>
    <img 
        src="./images/tableplus-mariadb.png" 
        alt="TablePlus settings for MariaDB"
        width="50%">
    </a>
    <br>
    <sub><sup>TablePlus settings for MariaDB.</sup></sub>
</p>

---

#### <a id="config-mongodb"></a> <ins>Configuring MongoDB</ins>

[MongoDB](https://www.mongodb.com/) is a source-available cross-platform 
document-oriented database program. Classified as a NoSQL database program, 
MongoDB uses JSON-like documents with optional schemas.

##### **DotEnv Variables**

The MongoDB Docker Compose file uses the follow variables from the DotEnv file.

```ini
#--------------------------------------------------------------------------
# MongoDB env
#--------------------------------------------------------------------------

MONGO_CONTAINER_NAME="${APP_NAME}_mongodb"

MONGO_DATABASE="${DB_DATABASE}"

MONGO_PORT=27017

MONGO_USERNAME="${DB_USERNAME}"
MONGO_PASSWORD="${DB_PASSWORD}"
```

**Please note:** You are unable to create additional users via the MongoDB 
Docker image environment variables. The Mongo username and password environment 
variables in the DotEnv are for the root user.

For a list of available environment variables that the MongoDB Docker image 
supports, you can visit [MongoDB Docker Hub](https://hub.docker.com/_/mongo) 
page.

##### **Start & Stop Docker container**

To start the MongoDB container, you can run the following command:

```bash
docker compose -f compose.mongodb.yaml up -d
```

To stop the MongoDB container, you can run the following command:

```bash
docker compose -f compose.mongodb.yaml down
```

##### **Connect to Database**

To connect to your MongoDB container from your database client, you will 
need to provide the following settings:

```ini
URL=mongodb://${MONGO_USERNAME}:${MONGO_PASSWORD}@localhost:${MONGO_PORT}/
```

Below is a screenshot of the settings used in TablePlus:

<p align="center">
    <a>
    <img 
        src="./images/tableplus-mongodb.png" 
        alt="TablePlus settings for MongoDB" 
        width="50%">
    </a>
    <br>
    <sub><sup>TablePlus settings for MongoDB.</sup></sub>
</p>

---

#### <a id="config-mssql"></a> <ins>Configuring Microsoft SQL Server (MSSQL)</ins>

[Microsoft SQL Server (MSSQL)](https://www.microsoft.com/en-gb/sql-server/) is a 
relational database management system developed by Microsoft.

> **Note**
> : MSSQL is not support on Apple Silicon. There is an open 
[GitHub issue](https://github.com/microsoft/mssql-docker/issues/734).

##### **DotEnv Variables**

The MSSQL Docker Compose file uses the follow variables from the DotEnv file.

```ini
#--------------------------------------------------------------------------
# Microsoft SQL Server (MSSQL) env
#--------------------------------------------------------------------------

MSSQL_CONTAINER_NAME="${APP_NAME}_mssql"

MSSQL_PORT=1433

MSSQL_ROOT_PASSWORD="${DB_ROOT_PASSWORD}"

MSSQL_PID="Developer"
```

**Please note:** The MSSQL password needs to include at least 8 characters of 
at least three of these four categories: uppercase letters, lowercase letters, 
numbers and non-alphanumeric symbols.

For a list of available environment variables that the MSSQL Docker image 
supports, you can visit [SQL Docs](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-configure-environment-variables?view=sql-server-ver15) 
page.

##### **Start & Stop Docker container**

To start the MSSQL container, you can run the following command:

```bash
docker compose -f compose.mssql.yaml up -d
```

To stop the MSSQL container, you can run the following command:

```bash
docker compose -f compose.mssql.yaml down
```

##### **Connect to Database**

To connect to your MSSQL container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${MSSQL_PORT}"

USER="sa"
PASSWORD="${MSSQL_ROOT_PASSWORD}"
```

**Please note:** The `SA` user is the system administrator account on the MSSQL 
Server instance that's created during setup.

Below is a screenshot of the settings used in TablePlus:

<p align="center">
    <a>
    <img 
        src="./images/tableplus-mssql.png" 
        alt="TablePlus settings for MSSQL"
        width="50%">
    </a>
    <br>
    <sub><sup>TablePlus settings for MSSQL.</sup></sub>
</p>

---

#### <a id="config-mysql"></a> <ins>Configuring MySQL</ins>

[MySQL](https://www.mysql.com/) is the world's most popular open-source 
relational database management system (RDBMS).

##### **DotEnv Variables**

The MySQL Docker Compose file uses the follow variables from the DotEnv file.

```ini
#--------------------------------------------------------------------------
# MySQL env
#--------------------------------------------------------------------------

MYSQL_CONTAINER_NAME="${APP_NAME}_mysql"

MYSQL_DATABASE="${DB_DATABASE}"

MYSQL_PORT="${DB_PORT}"

MYSQL_ROOT_PASSWORD="${DB_ROOT_PASSWORD}"

MYSQL_USERNAME="${DB_USERNAME}"
MYSQL_PASSWORD="${DB_PASSWORD}"
```

**Please note:** MySQL allows root's password to be empty.

For a list of available environment variables that the MySQL Docker image 
supports, you can visit [MySQL Docker Hub](https://hub.docker.com/_/mysql) 
page.

##### **Start & Stop Docker container**

To start the MySQL container, you can run the following command:

```bash
docker compose -f compose.mysql.yaml up -d
```

To stop the MySQL container, you can run the following command:

```bash
docker compose -f compose.mysql.yaml down
```

##### **Connect to Database**

To connect to your MySQL container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${MYSQL_PORT}"

USER="${MYSQL_USERNAME}"
PASSWORD="${MYSQL_PASSWORD}"
```

Below is a screenshot of the settings used in TablePlus:

<p align="center">
    <a>
    <img 
        src="./images/tableplus-mysql.png" 
        alt="TablePlus settings for MySQL"
        width="50%">
    </a>
    <br>
    <sub><sup>TablePlus settings for MySQL.</sup></sub>
</p>

---

#### <a id="config-postgres"></a> <ins>Configuring PostgreSQL</ins>

[PostgreSQL](https://www.postgresql.org/), also known as Postgres, is a free 
and open-source relational database management system emphasizing extensibility 
and SQL compliance.

##### **Environment Variables**

The PostgreSQL Docker Compose file uses the follow variables from the DotEnv 
file.

```ini
#--------------------------------------------------------------------------
# PostgreSQL env
#--------------------------------------------------------------------------

PGSQL_CONTAINER_NAME="${APP_NAME}_pgsql"

PGSQL_DATABASE="${DB_DATABASE}"

PGSQL_PORT=5432

PGSQL_USERNAME="${DB_USERNAME}"
PGSQL_PASSWORD="${DB_PASSWORD}"
```

For a list of available environment variables that the PostgreSQL Docker image 
supports, you can visit [PostgreSQL Docker Hub](https://hub.docker.com/_/postgres) 
page.

##### **Start & Stop Docker container**

To start the PostgreSQL container, you can run the following command:

```bash
docker compose -f compose.pgsql.yaml up -d
```

To stop the PostgreSQL container, you can run the following command:

```bash
docker compose -f compose.pgsql.yaml down
```

##### **Connect to Database**

To connect to your PostgreSQL container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${PGSQL_PORT}"

USER="${PGSQL_USERNAME}"
PASSWORD="${PGSQL_PASSWORD}"

DATABASE="${PGSQL_DATABASE}"
```

Below is a screenshot of the settings used in TablePlus:

<p align="center">
    <a>
    <img 
        src="./images/tableplus-pgsql.png" 
        alt="TablePlus settings for PostgreSQL"
        width="50%">
    </a>
    <br>
    <sub><sup>TablePlus settings for PostgreSQL.</sup></sub>
</p>

---

#### <a id="config-redis"></a> <ins>Configuring Redis</ins>

[Redis](https://redis.io/) is an in-memory data structure store, used as a 
distributed, in-memory key–value database, cache and message broker, with 
optional durability.

##### **Environment Variables**

The Redis Docker Compose file uses the follow variables from the DotEnv file.

```ini
#--------------------------------------------------------------------------
# Redis env
#--------------------------------------------------------------------------

REDIS_CONTAINER_NAME="${APP_NAME}_redis"

REDIS_PORT=6379
```

**Please note:** The Redis Docker image doesn't offer additional environment 
variables.

##### **Start & Stop Docker container**

To start the Redis container, you can run the following command:

```bash
docker compose -f compose.redis.yaml up -d
```

To stop the Redis container, you can run the following command:

```bash
docker compose -f compose.redis.yaml down
```

##### **Connect to Database**

To connect to your Redis container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${REDIS_PORT}"
```

Below is a screenshot of the settings used in TablePlus:

<p align="center">
    <a>
    <img 
        src="./images/tableplus-redis.png" 
        alt="TablePlus settings for Redis"
        width="50%">
    </a>
    <br>
    <sub><sup>TablePlus settings for Redis.</sup></sub>
</p>

---

#### <a id="config-surrealdb"></a> <ins>Configuring SurrealDB</ins>

[SurrealDB](https://surrealdb.com/), is an innovative NewSQL cloud database 
suitable for serverless, jamstack, single-page, and traditional applications.

##### **Environment Variables**

The SurrealDB Docker Compose file uses the follow variables from the DotEnv 
file.

```ini
#--------------------------------------------------------------------------
# SurrealDB env
#--------------------------------------------------------------------------

SURREALDB_CONTAINER_NAME="${APP_NAME}_surrealdb"

SURREALDB_PORT=8000

SURREALDB_USERNAME="${DB_USERNAME}"
SURREALDB_PASSWORD="${DB_PASSWORD}"
```

**Please note:** The SurrealDB Docker image doesn't offer additional environment 
variables.

##### **Start & Stop Docker container**

To start the SurrealDB container, you can run the following command:

```bash
docker compose -f compose.surrealdb.yaml up -d
```

To stop the SurrealDB container, you can run the following command:

```bash
docker compose -f compose.surrealdb.yaml down
```

##### **Connect to Database**

> **Note**
> : TablePlus currently doesn't support SurrealDB. You can use Postman to run 
queries on SurrealDB.

You will need to provide the following settings for your HTTP Request:

```ini
PORT="${SURREALDB_PORT}"

USER="${SURREALDB_USERNAME}"
PASSWORD="${SURREALDB_PASSWORD}"
```

Below is a cURL request using the settings:

```bash
curl --request POST \
	--header "Content-Type: application/json" \
	--header "NS: test" \
	--header "DB: test" \
	--user "${USERNAME}:${PASSWORD}" \
	--data "INFO FOR DB;" \
	http://localhost:${PORT}/sql
```

Below is a screenshot of the settings used in Postman:

<p align="center">
    <a>
    <img 
        src="./images/postman-surrealdb.png" 
        alt="Postman settings for SurrealDB"
        width="75%">
    </a>
    <br>
    <sub><sup>Postman settings for SurrealDB.</sup></sub>
</p>

---

#### <a id="config-yugabytedb"></a> <ins>Configuring YugabyteDB</ins>

[YugabyteDB](https://www.yugabyte.com/) is a high-performance distributed SQL 
database for global, internet-scale apps that need absolute data correctness. 
It aims to support all PostgreSQL features.

> **Note**
> : YugabyteDB supports PostgreSQL and Cassandra authentication and connection.

##### **Environment Variables**

The YugabyteDB Docker Compose file uses the follow variables from the DotEnv 
file.

```ini
#--------------------------------------------------------------------------
# YugabyteDB env
#--------------------------------------------------------------------------

YUGABYTEDB_CONTAINER_NAME="${APP_NAME}_yugabytedb"

YUGABYTEDB_YSQL_PORT=5433
YUGABYTEDB_YCQL_PORT=9042

YUGABYTEDB_MASTER_PORT=7001
YUGABYTEDB_TSERVER_PORT=9000
```

**Please note:** The YugabyteDB Docker image doesn't offer additional 
environment variables. You are unable to create additional users via the 
YugabyteDB Docker image environment variables.

**Please note:** MacOS Monterey enables AirPlay receiving by default, which 
listens on port 7000. This conflicts with YugabyteDB default YB-Master port.

##### **Start & Stop Docker container**

To start the YugabyteDB container, you can run the following command:

```bash
docker compose -f compose.yugabytedb.yaml up -d
```

To stop the YugabyteDB container, you can run the following command:

```bash
docker compose -f compose.yugabytedb.yaml down
```

##### **Connect to Database (Admin UI)**

YugabyteDB cluster consists of two processes: YB-Master which keeps track of 
various metadata (list of tables, users, roles, permissions, and so on) and 
YB-TServer which is responsible for the actual end user requests for data 
updates and queries.

Each of the processes exposes its own Admin UI that can be used to check the 
status of the corresponding process, as well as perform certain administrative 
operations. The YB-Master Admin UI is available at http://localhost:7001 and 
the YB-TServer Admin UI is available at http://localhost:9000.

##### **Connect to Database (PostgreSQL settings)**

To connect to your YugabyteDB container from your database client using 
PostgreSQL settings, you will need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${YUGABYTEDB_YSQL_PORT}"

USER="yugabyte"
PASSWORD="yugabyte"
```

Please note: The `yugabyte` user is the system administrator account on the 
YugabyteDB Server instance that's created during setup.

Below is a screenshot of the settings used in TablePlus:

<p align="center">
    <a>
    <img 
        src="./images/tableplus-yugabytedb-pgsql.png" 
        alt="TablePlus settings for YugabyteDB"
        width="50%">
    </a>
    <br>
    <sub><sup>TablePlus settings for YugabyteDB (PostgreSQL settings).</sup></sub>
</p>

##### **Connect to Database (Cassandra settings)**

To connect to your YugabyteDB container from your database client using 
Cassandra settings, you will need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${YUGABYTEDB_YCQL_PORT}"

USER="cassandra"
PASSWORD="cassandra"
```

Please note: The `cassandra` user is the system administrator account on the 
YugabyteDB Server instance that's created during setup.

Below is a screenshot of the settings used in TablePlus:

<p align="center">
    <a>
    <img 
        src="./images/tableplus-yugabytedb-cassandra.png" 
        alt="TablePlus settings for YugabyteDB"
        width="50%">
    </a>
    <br>
    <sub><sup>TablePlus settings for YugabyteDB (Cassandra settings).</sup></sub>
</p>

## Docker Network

If you wish to attach your Docker containers to the database network to allow 
other containers to access your database. I have outlined the necessary 
configuration below both for Docker Compose and Docker CLI approach.

Once your database container is up and running, you will need to configure your 
containers by attaching the **local_dbs_network** network to your container(s).

**Docker Compose**

In your Docker Compose file you need to define **local_dbs_network** as an 
external network. For each services you want to have access to your database 
container, you will need to add **local_dbs_network** as an attached network.

##### **Docker Compose**

```yaml
version: '3.8'

services:
  backend:
    container_name: backend
    image: python:3
    # (Optional) depends on database container name
    depends_on:
      - local_dbs_mongodb
    # Add local_dbs_network as attached network.
    networks:
      - local_dbs_network
    volumes:
      - ./:/usr/src/myapp 
    command: ["python" "main.py"]

networks:
  # Add local_dbs_network as an external network.
  local_dbs_network:
    external: true
```

**Docker CLI**

If you don't use Docker Compose, I have included an example of Docker CLI to 
start a container with the necessary configurations.

```bash
$ docker run --rm -it --name backend --network=local_dbs_network -v "$PWD":/usr/src/myapp -w /usr/src/myapp python:3 python main.py
```

## FAQ

**Q:** Are you planning to add additional databases, e.g., CouchDB & Cassandra?  
**A:** I don't have a roadmap for adding additional databases to this repository. 
However, you can suggest a database in the 
[Discussion section](https://github.com/luisaveiro/localhost-databases/discussions/categories/ideas) 
and I will try to include the database as part of the repository's database 
collection.

## Useful Tips

[NoSQL Workbench](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/workbench.html)
is an Amazon DynamoDB client that provides data modeling, data visualization, 
and query development.

[TablePlus](https://tableplus.com/) is a modern, native tool for database 
management that supports whole set of relational databases (and some NoSQL).

[Postman](https://www.postman.com/) enables you to easily explore, debug, and 
test your APIs while also enabling you to define complex API requests for HTTP, 
REST, SOAP, GraphQL, and WebSockets.

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Contributing

I encourage you to contribute to ***Localhost Databases***! Contributions are 
what make the open source community such an amazing place to be learn, inspire, 
and create. Any contributions you make are **greatly appreciated**.

Please check out the [contributing to Localhost Databases guide](.github/CONTRIBUTING.md) 
for guidelines about how to proceed.

## Security Vulnerabilities

Trying to report a possible security vulnerability in ***Localhost Databases***? 
Please check out our [security policy](.github/SECURITY.md) for guidelines 
about how to proceed.

## Credits

The illustration used in the project is from [unDraw (created by Katerina Limpitsouni)](https://undraw.co/). 
All product names, logos, brands, trademarks and registered trademarks are 
property of their respective owners.

## Sponsor

Do you like this project? Support it by donating.

<a href="https://www.buymeacoffee.com/luisaveiro">
  <img src="./images/bmc-button.svg" alt="Code Review" width="144">
</a>

## License

The MIT License (MIT). Please see [License File](LICENSE) for more information.

---

<p align="center">
  <a href="http://github.com/luisaveiro" target="_blank">GitHub</a> •
  <a href="https://uk.linkedin.com/in/luisaveiro" target="_blank">LinkedIn</a> •
  <a href="https://twitter.com/luisdeaveiro" target="_blank">Twitter</a>
</p>
