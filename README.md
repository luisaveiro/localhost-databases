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
    <a href="#tldr">TL;DR</a> •
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

## <a id="tldr"></a> TL;DR

Want to learn or experiment with different database engines without requiring 
to install additional dependencies? ***Localhost Databases*** is a collection 
of Docker Compose files for relational and NoSQL databases.

#### Quick Start

1. Clone this repository.
2. The Database Docker Compose files are located in the `databases` folder.
2. Copy the DotEnv example file to create your DotEnv file and configure your 
database's credentials and settings.
3. Starting a database Docker container is simple by running the `docker compose up` command.

##### Docker Compose Command:

```bash
$ docker compose up -d
```

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

***Localhost Databases*** includes a `.env.example` file for each Database. You 
can run the following command in the terminal to create your DotEnv file.

```bash
# Navigate to a database.
$ cd databases/mysql

# Create .env from .env.example.
$ cp .env.example .env
```

Each database has its environment variables (below, I have provided more information). 
You have the option to modify each of the database's environment variables 
individually.

```ini
#--------------------------------------------------------------------------
# Database (MySQL) env
#--------------------------------------------------------------------------

# The MySQL database container name. | default: mysql_db
DB_CONTAINER_NAME="${APP_NAME}_db"

# The MySQL database configuration. | default: local
DB_DATABASE="local"

# The MySQL database root credentials.
DB_ROOT_PASSWORD=""

# The MySQL database user credentials.
DB_USERNAME=""
DB_PASSWORD=""

# Map the database container exposed port to the host port. | default: 3306
DB_PORT=3306
```

#### 2. <ins>Start database container</ins>

After you configure your DotEnv, you can start a database container. Each 
database has its individual Docker Compose file. You can run the Docker Compose
`up` command.

```bash
$ docker compose up -d
```

An example of the `docker compose` command would be as follows:

```bash
# Navigate to a database.
$ cd databases/redis

# Run Docker Compose command.
$ docker compose up -d
```

Docker will create the database container with the container name 
`redis_db` in our example. The container will be attached to a network 
called `local_dbs_network`.

If you want to change the container name or network name, you can edit the 
DotEnv file and override the Docker Compose variables. Below is an example of 
the DotEnv variables.

```ini
#--------------------------------------------------------------------------
# Database (Redis) env
#--------------------------------------------------------------------------

# The Redis database container name. | default: redis_db
DB_CONTAINER_NAME="redis_db"

#--------------------------------------------------------------------------
# Network env
#--------------------------------------------------------------------------

# The Docker network for the containers. | default: local_dbs_network
NETWORK_NAME="local_dbs_network"
```

## Databases

Localhost Databases include 13 database servers. The following databases are 
part of this repository's collection:

- [Cassandra](#config-cassandra)
- [CockroachDB](#config-cockroachdb)
- [CouchDB](#config-couchdb)
- [DynamoDB Local](#config-dynamodb)
- [EdgeDB](#config-edgedb)
- [MariaDB](#config-mariadb)
- [MongoDB](#config-mongodb)
- [Microsoft SQL Server (MSSQL)](#config-mssql)
- [MySQL](#config-mysql)
- [Neo4j](#config-neo4j)
- [PostgreSQL](#config-postgres)
- [Redis](#config-redis)
- [SurrealDB](#config-surrealdb)
- [YugabyteDB](#config-yugabytedb)

Below I have provided more information on how to configure each database, 
start the database container and connect to the database via a database client 
app.

---
<details>
<summary><a id="config-cassandra"><b>Cassandra</b></a></summary>

#### <ins>Configuring Cassandra</ins>

[Apache Cassandra](https://cassandra.apache.org) is an open source NoSQL 
distributed database that manages large amounts of data across commodity 
servers. It is a decentralized, scalable storage system designed to handle vast 
volumes of data across multiple commodity servers, providing high availability 
without a single point of failure.

You can learn about [Cassandra in 100 Seconds](https://youtu.be/ziq7FUKpCS8) by 
watching [Fireship YouTube channel](https://www.youtube.com/@Fireship).

##### **Environment Variables**

The Cassandra Docker Compose file uses the follow variables from the DotEnv 
file.

```ini
#--------------------------------------------------------------------------
# Docker env
#--------------------------------------------------------------------------

# The project name. | default: cassandra
APP_NAME="cassandra"

#--------------------------------------------------------------------------
# Database (Cassandra) env
#--------------------------------------------------------------------------

# The Cassandra database container name. | default: cassandra_db
DB_CONTAINER_NAME="${APP_NAME}_db"

#--------------------------------------------------------------------------
# Network env
#--------------------------------------------------------------------------

# Map the database container exposed port to the host port. | default: 9042
DB_PORT=9042

# The Docker network for the containers. | default: local_dbs_network
NETWORK_NAME="local_dbs_network"

#--------------------------------------------------------------------------
# Volume env
#--------------------------------------------------------------------------

# The database container data volume. | default: cassandra_db_data
DB_VOLUME_DATA_NAME="${DB_CONTAINER_NAME}_data"
```

**Please note:** You are unable to create additional users via the Cassandra 
Docker image environment variables.

##### **Start & Stop Docker container**

To start the Cassandra Local container, you can run the following command:

```bash
# Navigate to Cassandra database.
$ cd databases/cassandra

# Run Docker Compose command.
$ docker compose up -d
```

To stop the Cassandra Local container, you can run the following command:

```bash
$ docker compose down
```

##### **Connect to Database**

To connect to your Cassandra container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${DB_PORT}"

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

</details>

---

<details>
<summary><a id="config-cockroachdb"><b>CockroachDB</b></a></summary>

#### <ins>Configuring CockroachDB</ins>

[CockroachDB](https://www.cockroachlabs.com/) is a distributed database with 
standard SQL for cloud applications.

##### **Environment Variables**

The CockroachDB Docker Compose file uses the follow variables from the DotEnv 
file.

```ini
#--------------------------------------------------------------------------
# Docker env
#--------------------------------------------------------------------------

# The project name. | default: cockroachdb
APP_NAME="cockroachdb"

#--------------------------------------------------------------------------
# Database (CockroachDB) env
#--------------------------------------------------------------------------

# The CockroachDB database container name. | default: cockroachdb
DB_CONTAINER_NAME="${APP_NAME}"

#--------------------------------------------------------------------------
# Network env
#--------------------------------------------------------------------------

# Map the database container exposed port to the host port. | default: 26257
DB_PORT=26257

# Map the database container UI port to the host port. | default: 8080
DB_UI_PORT=8080

# The Docker network for the containers. | default: local_dbs_network
NETWORK_NAME="local_dbs_network"

#--------------------------------------------------------------------------
# Volume env
#--------------------------------------------------------------------------

# The database container data volume. | default: cockroachdb_data
DB_VOLUME_DATA_NAME="${DB_CONTAINER_NAME}_data"
```

##### **Start & Stop Docker container**

To start the CockroachDB Local container, you can run the following command:

```bash
# Navigate to CockroachDB database.
$ cd databases/cockroachdb

# Run Docker Compose command.
$ docker compose up -d
```

To stop the CockroachDB Local container, you can run the following command:

```bash
$ docker compose down
```

##### **Connect to Database**

To connect to your CockroachDB container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${DB_PORT}"

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

</details>

---

<details>
<summary><a id="config-couchdb"><b>CouchDB</b></a></summary>

#### <ins>CouchDB</ins>

[Apache CouchDB](https://couchdb.apache.org/) is an open-source document-oriented 
NoSQL database, implemented in Erlang. CouchDB uses multiple formats and protocols 
to store, transfer, and process its data. It uses JSON to store data, JavaScript 
as its query language using MapReduce, and HTTP for an API.

##### **Environment Variables**

The CouchDB Local Docker Compose file uses the follow variables from the DotEnv 
file.

```ini
#--------------------------------------------------------------------------
# Docker env
#--------------------------------------------------------------------------

# The project name. | default: couchdb
APP_NAME="couchdb"

#--------------------------------------------------------------------------
# Database (CouchDB) env
#--------------------------------------------------------------------------

# The CouchDB database container name. | default: couchdb
DB_CONTAINER_NAME="${APP_NAME}"

# The CouchDB database user credentials.
DB_USERNAME=""
DB_PASSWORD=""

#--------------------------------------------------------------------------
# Network env
#--------------------------------------------------------------------------

# Map the database container exposed port to the host port. | default: 5984
DB_PORT=5984

# The Docker network for the containers. | default: local_dbs_network
NETWORK_NAME="local_dbs_network"

#--------------------------------------------------------------------------
# Volume env
#--------------------------------------------------------------------------

# The database container data volume. | default: couchdb_data
DB_VOLUME_DATA_NAME="${DB_CONTAINER_NAME}_data"
```

##### **Start & Stop Docker container**

To start the CouchDB Local container, you can run the following command:

```bash
# Navigate to CouchDB database.
$ cd databases/couchdb

# Run Docker Compose command.
$ docker compose up -d
```

To stop the CouchDB Local container, you can run the following command:

```bash
$ docker compose down
```

##### **Connect to Database**

Fauxton is a native web-based interface built into CouchDB. It provides a basic 
interface to the majority of the functionality, including the ability to create, 
update, delete and view documents and design documents. It provides access to 
the configuration parameters, and an interface for initiating replication.To 
connect to your EdgeDB CouchDB from your browser, you will need to provide the 
following settings:

```ini
HOST=localhost
PORT="${DB_PORT}"

USER="${DB_USERNAME}"
PASSWORD="${DB_PASSWORD}"
```

Go to http://localhost:5984/_utils. Below is a screenshot of the Fauxton UI:

<p align="center">
    <a>
    <img 
        src="./images/browser-couchdb.png" 
        alt="CouchDB Fauxton UI"
        width="50%">
    </a>
    <br>
    <sub><sup>CouchDB Fauxton UI.</sup></sub>
</p>

</details>

---

<details>
<summary><a id="config-dynamodb"><b>DynamoDB Local</b></a></summary>

#### <ins>Configuring DynamoDB Local</ins>

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
# Docker env
#--------------------------------------------------------------------------

# The project name. | default: dynamodb
APP_NAME="dynamodb"

#--------------------------------------------------------------------------
# Database (DynamoDB) env
#--------------------------------------------------------------------------

# The DynamoDB database container name. | default: dynamodb
DB_CONTAINER_NAME="${APP_NAME}"

#--------------------------------------------------------------------------
# Network env
#--------------------------------------------------------------------------

# Map the database container exposed port to the host port. | default: 8000
DB_PORT=8000

# The Docker network for the containers. | default: local_dbs_network
NETWORK_NAME="local_dbs_network"

#--------------------------------------------------------------------------
# Volume env
#--------------------------------------------------------------------------

# The database container data volume. | default: dynamodb_data
DB_VOLUME_DATA_NAME="${DB_CONTAINER_NAME}_data"
```

##### **Start & Stop Docker container**

To start the DynamoDB Local container, you can run the following command:

```bash
# Navigate to DynamoDB database.
$ cd databases/dynamodb

# Run Docker Compose command.
$ docker compose up -d
```

To stop the DynamoDB Local container, you can run the following command:

```bash
$ docker compose down
```

##### **Connect to Database**

> **Note**
> : TablePlus currently doesn't support DynamoDB. You can use NoSQL Workbench.

To connect to your DynamoDB Local container from your database client, you will 
need to provide the following settings:

```ini
HOST=localhost
PORT="${DB_PORT}"
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

</details>

---

<details>
<summary><a id="config-edgedb"><b>EdgeDB</b></a></summary>

#### <ins>Configuring EdgeDB</ins>

[EdgeDB](https://www.edgedb.com) is an open-source database designed as a 
spiritual successor to SQL and the relational paradigm. Powered by the Postgres 
query engine.

##### **Environment Variables**

The EdgeDB Docker Compose file uses the follow variables from the DotEnv file.

```ini
#--------------------------------------------------------------------------
# Docker env
#--------------------------------------------------------------------------

# The project name. | default: edgedb
APP_NAME="edgedb"

#--------------------------------------------------------------------------
# Database (EdgeDB) env
#--------------------------------------------------------------------------

# The EdgeDB database container name. | default: edgedb
DB_CONTAINER_NAME="${APP_NAME}"

# The EdgeDB database root credentials.
DB_ROOT_PASSWORD=""

#--------------------------------------------------------------------------
# Network env
#--------------------------------------------------------------------------

# Map the database container exposed port to the host port. | default: 5656
DB_PORT=5656

# The Docker network for the containers. | default: local_dbs_network
NETWORK_NAME="local_dbs_network"

#--------------------------------------------------------------------------
# Volume env
#--------------------------------------------------------------------------

# The database container data volume. | default: edgedb_data
DB_VOLUME_DATA_NAME="${DB_CONTAINER_NAME}_data"

# The database container schema volume. | default: edgedb_schema
DB_VOLUME_SCHEMA_NAME="${DB_CONTAINER_NAME}_schema"
```

**Please note:** EdgeDB root username is `edgedb`.

For a list of available environment variables that the EdgeDB Docker image 
supports, you can visit [EdgeDB Documentation](https://www.edgedb.com/docs/reference/environment#ref-reference-environment) 
page.

##### **Start & Stop Docker container**

To start the EdgeDB container, you can run the following command:

```bash
# Navigate to EdgeDB database.
$ cd databases/edgedb

# Run Docker Compose command.
$ docker compose up -d
```

To stop the EdgeDB container, you can run the following command:

```bash
$ docker compose down
```

##### **Connect to Database**

EdgeDB Admin UI is a developer-focused tool that allows you to execute EdgeQL 
queries and visualize the results. To connect to your EdgeDB container from your 
browser, you will need to provide the following settings:

```ini
HOST=localhost
PORT="${DB_PORT}"

USER="edgedb"
PASSWORD="${DB_ROOT_PASSWORD}"
```

Go to http://localhost:5656. Below is a screenshot of the EdgeDB Admin UI:

<p align="center">
    <a>
    <img 
        src="./images/browser-edgedb.png" 
        alt="EdgeDB Admin UI"
        width="50%">
    </a>
    <br>
    <sub><sup>EdgeDB Admin UI.</sup></sub>
</p>

</details>

---

<details>
<summary><a id="config-mariadb"><b>MariaDB</b></a></summary>

#### <ins>Configuring MariaDB</ins>

[MariaDB](https://mariadb.org/) Server is one of the most popular open source 
relational databases. It's made by the original developers of MySQL.

##### **Environment Variables**

The MariaDB Docker Compose file uses the follow variables from the DotEnv file.

```ini
#--------------------------------------------------------------------------
# Docker env
#--------------------------------------------------------------------------

# The project name. | default: mariadb
APP_NAME="mariadb"

#--------------------------------------------------------------------------
# Database (MariaDB) env
#--------------------------------------------------------------------------

# The MariaDB database container name. | default: mariadb
DB_CONTAINER_NAME="${APP_NAME}"

# The MariaDB database configuration. | default: local
DB_DATABASE=local

# The MariaDB database root credentials.
DB_ROOT_PASSWORD=""

# The MariaDB database user credentials.
DB_USERNAME=""
DB_PASSWORD=""

#--------------------------------------------------------------------------
# Network env
#--------------------------------------------------------------------------

# Map the database container exposed port to the host port. | default: 3306
DB_PORT=3306

# The Docker network for the containers. | default: local_dbs_network
NETWORK_NAME="local_dbs_network"

#--------------------------------------------------------------------------
# Volume env
#--------------------------------------------------------------------------

# The database container data volume. | default: mariadb_data
DB_VOLUME_DATA_NAME="${DB_CONTAINER_NAME}_data"
```

**Please note:** MariaDB allows root's password to be empty.

For a list of available environment variables that the MariaDB Docker image 
supports, you can visit [MariaDB Docker Hub](https://hub.docker.com/_/mariadb) 
page.

##### **Start & Stop Docker container**

To start the MariaDB container, you can run the following command:

```bash
# Navigate to MariaDB database.
$ cd databases/mariadb

# Run Docker Compose command.
$ docker compose up -d
```

To stop the MariaDB container, you can run the following command:

```bash
$ docker compose down
```

##### **Connect to Database**

To connect to your MariaDB container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${DB_PORT}"

USER="${DB_USERNAME}"
PASSWORD="${DB_PASSWORD}"
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

</details>

---

<details>
<summary><a id="config-mongodb"><b>MongoDB</b></a></summary>

#### <ins>Configuring MongoDB</ins>

[MongoDB](https://www.mongodb.com/) is a source-available cross-platform 
document-oriented database program. Classified as a NoSQL database program, 
MongoDB uses JSON-like documents with optional schemas.

You can learn about [MongoDB in 100 Seconds](https://youtu.be/-bt_y4Loofg) by 
watching [Fireship YouTube channel](https://www.youtube.com/@Fireship).

##### **DotEnv Variables**

The MongoDB Docker Compose file uses the follow variables from the DotEnv file.

```ini
#--------------------------------------------------------------------------
# Docker env
#--------------------------------------------------------------------------

# The project name. | default: mongodb
APP_NAME="mongodb"

#--------------------------------------------------------------------------
# Database (MongoDB) env
#--------------------------------------------------------------------------

# The MongoDB database container name. | default: mongodb
DB_CONTAINER_NAME="${APP_NAME}"

# The MariaDB database configuration. | default: local
DB_DATABASE=local

# The MongoDB database user credentials.
DB_USERNAME=""
DB_PASSWORD=""

#--------------------------------------------------------------------------
# Network env
#--------------------------------------------------------------------------

# Map the database container exposed port to the host port. | default: 27017
DB_PORT=27017

# The Docker network for the containers. | default: local_dbs_network
NETWORK_NAME="local_dbs_network"

#--------------------------------------------------------------------------
# Volume env
#--------------------------------------------------------------------------

# The database container config volume. | default: mongodb_config
DB_VOLUME_CONFIG_NAME="${DB_CONTAINER_NAME}_config"

# The database container data volume. | default: mongodb_data
DB_VOLUME_DATA_NAME="${DB_CONTAINER_NAME}_data"
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
# Navigate to MongoDB database.
$ cd databases/mongodb

# Run Docker Compose command.
$ docker compose up -d
```

To stop the MongoDB container, you can run the following command:

```bash
$ docker compose down
```

##### **Connect to Database**

To connect to your MongoDB container from your database client, you will 
need to provide the following settings:

```ini
URL=mongodb://${DB_USERNAME}:${DB_PASSWORD}@localhost:${DB_PORT}/
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

</details>

---

<details>
<summary><a id="config-mssql"><b>Microsoft SQL Server (MSSQL)</b></a></summary>

#### <ins>Configuring Microsoft SQL Server (MSSQL)</ins>

[Microsoft SQL Server (MSSQL)](https://www.microsoft.com/en-gb/sql-server/) is a 
relational database management system developed by Microsoft.

> **Note**
> : MSSQL is not support on Apple Silicon. There is an open 
[GitHub issue](https://github.com/microsoft/mssql-docker/issues/734).

##### **DotEnv Variables**

The MSSQL Docker Compose file uses the follow variables from the DotEnv file.

```ini
#--------------------------------------------------------------------------
# Docker env
#--------------------------------------------------------------------------

# The project name. | default: mssql
APP_NAME="mssql"

#--------------------------------------------------------------------------
# Database (Microsoft SQL Server - MSSQL) env
#--------------------------------------------------------------------------

# The MSSQL database container name. | default: mssql_db
DB_CONTAINER_NAME="${APP_NAME}_db"

# The MSSQL database root credentials.
DB_ROOT_PASSWORD=""

# The Product ID (PID) or Edition | default: Developer
MSSQL_PID="Developer"

#--------------------------------------------------------------------------
# Network env
#--------------------------------------------------------------------------

# Map the database container exposed port to the host port. | default: 1433
DB_PORT=1433

# The Docker network for the containers. | default: local_dbs_network
NETWORK_NAME="local_dbs_network"

#--------------------------------------------------------------------------
# Volume env
#--------------------------------------------------------------------------

# The database container data volume. | default: mssql_db_data
DB_VOLUME_DATA_NAME="${DB_CONTAINER_NAME}_data"
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
# Navigate to MSSQL database.
$ cd databases/mssql

# Run Docker Compose command.
$ docker compose up -d
```

To stop the MSSQL container, you can run the following command:

```bash
$ docker compose down
```

##### **Connect to Database**

To connect to your MSSQL container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${DB_PORT}"

USER="sa"
PASSWORD="${DB_ROOT_PASSWORD}"
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

</details>

---

<details>
<summary><a id="config-mysql"><b>MySQL</b></a></summary>

#### <ins>Configuring MySQL</ins>

[MySQL](https://www.mysql.com/) is the world's most popular open-source 
relational database management system (RDBMS).

##### **DotEnv Variables**

The MySQL Docker Compose file uses the follow variables from the DotEnv file.

```ini
#--------------------------------------------------------------------------
# Docker env
#--------------------------------------------------------------------------

# The project name. | default: mysql
APP_NAME="mysql"

#--------------------------------------------------------------------------
# Database (MySQL) env
#--------------------------------------------------------------------------

# The MySQL database container name. | default: mysql_db
DB_CONTAINER_NAME="${APP_NAME}_db"

# The MySQL database configuration. | default: local
DB_DATABASE="local"

# The MySQL database root credentials.
DB_ROOT_PASSWORD=""

# The MySQL database user credentials.
DB_USERNAME=""
DB_PASSWORD=""

#--------------------------------------------------------------------------
# Network env
#--------------------------------------------------------------------------

# Map the database container exposed port to the host port. | default: 3306
DB_PORT=3306

# The Docker network for the containers. | default: local_dbs_network
NETWORK_NAME="local_dbs_network"

#--------------------------------------------------------------------------
# Volume env
#--------------------------------------------------------------------------

# The database container data volume. | default: mysql_db_data
DB_VOLUME_DATA_NAME="${DB_CONTAINER_NAME}_data"
```

**Please note:** MySQL allows root's password to be empty.

For a list of available environment variables that the MySQL Docker image 
supports, you can visit [MySQL Docker Hub](https://hub.docker.com/_/mysql) 
page.

##### **Start & Stop Docker container**

To start the MySQL container, you can run the following command:

```bash
# Navigate to MySQL database.
$ cd databases/mysql

# Run Docker Compose command.
$ docker compose up -d
```

To stop the MySQL container, you can run the following command:

```bash
$ docker compose down
```

##### **Connect to Database**

To connect to your MySQL container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${DB_PORT}"

USER="${DB_USERNAME}"
PASSWORD="${DB_PASSWORD}"
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

</details>

---

<details>
<summary><a id="config-neo4j"><b>Neo4j</b></a></summary>

#### <ins>Configuring Neo4j</ins>

[Neo4j](https://neo4j.com/) is an open-source, NoSQL, native graph database 
management system that provides an ACID-compliant transactional backend for 
your applications that has been publicly available since 2007.

You can learn about [Neo4j in 100 Seconds](https://youtu.be/T6L9EoBy8Zk) by 
watching [Fireship YouTube channel](https://www.youtube.com/@Fireship).

##### **Environment Variables**

The Neo4j Docker Compose file uses the follow variables from the DotEnv file.

```ini
#--------------------------------------------------------------------------
# Docker env
#--------------------------------------------------------------------------

# The project name. | default: neo4j
APP_NAME="neo4j"

#--------------------------------------------------------------------------
# Database (Neo4j) env
#--------------------------------------------------------------------------

# The Neo4j database container name. | default: neo4j_db
DB_CONTAINER_NAME="${APP_NAME}_db"

#--------------------------------------------------------------------------
# Network env
#--------------------------------------------------------------------------

# Map the database container exposed port to the host port. | default: 7474
DB_PORT=7474

# Map the database container exposed port to the host port. | default: 7687
DB_BOLT_PORT=7687

# The Docker network for the containers. | default: local_dbs_network
NETWORK_NAME="local_dbs_network"

#--------------------------------------------------------------------------
# Volume env
#--------------------------------------------------------------------------

# The database container data volume. | default: neo4j_db_data
DB_VOLUME_DATA_NAME="${DB_CONTAINER_NAME}_data"

# The database container logs volume. | default: neo4j_db_logs
DB_VOLUME_LOGS_NAME="${DB_CONTAINER_NAME}_data"
```

Neo4j configuration settings can be provided in the Neo4j Docker Compose file 
using the following naming scheme:

- Prefix with `NEO4J_`.
- Underscores must be written twice: `_` is written as `__`.
- Periods are converted to underscores: `.` is written as `_`.

As an example, `dbms.memory.pagecache.size` could be set by specifying the 
following argument in Neo4j Docker Compose file:

```yaml
services:
  neo4j:
    image: neo4j:latest
    environment:
      NEO4J_dbms_memory_pagecache_size=4G
```

For a list of available environment variables that the Neo4j Docker image 
supports, you can visit [Neo4j Configuration Settings](https://neo4j.com/docs/operations-manual/current/reference/configuration-settings/) 
page.

##### **Start & Stop Docker container**

To start the Neo4j container, you can run the following command:

```bash
# Navigate to Neo4j database.
$ cd databases/neo4j

# Run Docker Compose command.
$ docker compose up -d
```

To stop the Neo4j container, you can run the following command:

```bash
docker compose down
```

##### **Connect to Database**

Neo4j Browser is a developer-focused tool that allows you to execute Cypher 
queries and visualize the results. To connect to your Neo4j container from your 
browser, you will need to provide the following settings:

```ini
HOST=localhost
PORT="${DB_PORT}"

USER="neo4j"
PASSWORD="neo4j"
```

Go to http://localhost:7474. Below is a screenshot of the Neo4j Browser:

<p align="center">
    <a>
    <img 
        src="./images/browser-neo4j.png" 
        alt="Neo4j Browser"
        width="50%">
    </a>
    <br>
    <sub><sup>Neo4j Browser.</sup></sub>
</p>

</details>

---

<details>
<summary><a id="config-postgres"><b>PostgreSQL</b></a></summary>

#### <ins>Configuring PostgreSQL</ins>

[PostgreSQL](https://www.postgresql.org/), also known as Postgres, is a free 
and open-source relational database management system emphasizing extensibility 
and SQL compliance.

##### **Environment Variables**

The PostgreSQL Docker Compose file uses the follow variables from the DotEnv 
file.

```ini
#--------------------------------------------------------------------------
# Docker env
#--------------------------------------------------------------------------

# The project name. | default: pgsql
APP_NAME="pgsql"

#--------------------------------------------------------------------------
# Database (PostgreSQL) env
#--------------------------------------------------------------------------

# The PostgreSQL database container name. | default: pgsql_db
DB_CONTAINER_NAME="${APP_NAME}_db"

# The PostgreSQL database configuration. | default: local
DB_DATABASE="local"

# The PostgreSQL database user credentials.
DB_USERNAME=""
DB_PASSWORD=""

#--------------------------------------------------------------------------
# Network env
#--------------------------------------------------------------------------

# Map the database container exposed port to the host port. | default: 5432
DB_PORT=5432

# The Docker network for the containers. | default: local_dbs_network
NETWORK_NAME="local_dbs_network"

#--------------------------------------------------------------------------
# Volume env
#--------------------------------------------------------------------------

# The database container data volume. | default: pgsql_db_data
DB_VOLUME_DATA_NAME="${DB_CONTAINER_NAME}_data"
```

For a list of available environment variables that the PostgreSQL Docker image 
supports, you can visit [PostgreSQL Docker Hub](https://hub.docker.com/_/postgres) 
page.

##### **Start & Stop Docker container**

To start the PostgreSQL container, you can run the following command:

```bash
# Navigate to PostgreSQL database.
$ cd databases/pgsql

# Run Docker Compose command.
$ docker compose up -d
```

To stop the PostgreSQL container, you can run the following command:

```bash
$ docker compose down
```

##### **Connect to Database**

To connect to your PostgreSQL container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${DB_PORT}"

USER="${DB_USERNAME}"
PASSWORD="${DB_PASSWORD}"

DATABASE="${DB_DATABASE}"
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

</details>

---

<details>
<summary><a id="config-redis"><b>Redis</b></a></summary>

#### <ins>Configuring Redis</ins>

[Redis](https://redis.io/) is an in-memory data structure store, used as a 
distributed, in-memory key–value database, cache and message broker, with 
optional durability.

You can learn about [Redis in 100 Seconds](https://youtu.be/G1rOthIU-uo) by 
watching [Fireship YouTube channel](https://www.youtube.com/@Fireship).

##### **Environment Variables**

The Redis Docker Compose file uses the follow variables from the DotEnv file.

```ini
#--------------------------------------------------------------------------
# Docker env
#--------------------------------------------------------------------------

# The project name. | default: redis
APP_NAME="redis"

#--------------------------------------------------------------------------
# Database (Redis) env
#--------------------------------------------------------------------------

# The Redis database container name. | default: redis_db
DB_CONTAINER_NAME="${APP_NAME}_db"

#--------------------------------------------------------------------------
# Network env
#--------------------------------------------------------------------------

# Map the database container exposed port to the host port. | default: 6379
DB_PORT=6379

# The Docker network for the containers. | default: local_dbs_network
NETWORK_NAME="local_dbs_network"

#--------------------------------------------------------------------------
# Volume env
#--------------------------------------------------------------------------

# The database container data volume. | default: redis_db_data
DB_VOLUME_DATA_NAME="${DB_CONTAINER_NAME}_data"
```

**Please note:** The Redis Docker image doesn't offer additional environment 
variables.

##### **Start & Stop Docker container**

To start the Redis container, you can run the following command:

```bash
# Navigate to Redis database.
$ cd databases/redis

# Run Docker Compose command.
$ docker compose up -d
```

To stop the Redis container, you can run the following command:

```bash
$ docker compose down
```

##### **Connect to Database**

To connect to your Redis container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${DB_PORT}"
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

</details>

---

<details>
<summary><a id="config-surrealdb"><b>SurrealDB</b></a></summary>

#### <ins>Configuring SurrealDB</ins>

[SurrealDB](https://surrealdb.com/), is an innovative NewSQL cloud database 
suitable for serverless, jamstack, single-page, and traditional applications.

You can learn about [SurrealDB in 100 Seconds](https://youtu.be/C7WFwgDRStM) by 
watching [Fireship YouTube channel](https://www.youtube.com/@Fireship).

##### **Environment Variables**

The SurrealDB Docker Compose file uses the follow variables from the DotEnv 
file.

```ini
#--------------------------------------------------------------------------
# Docker env
#--------------------------------------------------------------------------

# The project name. | default: surrealdb
APP_NAME="surrealdb"

#--------------------------------------------------------------------------
# Database (SurrealDB) env
#--------------------------------------------------------------------------

# The SurrealDB database container name. | default: surrealdb
DB_CONTAINER_NAME="${APP_NAME}"

# The SurrealDB database user credentials.
DB_USERNAME=""
DB_PASSWORD=""

#--------------------------------------------------------------------------
# Network env
#--------------------------------------------------------------------------

# Map the database container exposed port to the host port. | default: 8000
DB_PORT=8000

# The Docker network for the containers. | default: local_dbs_network
NETWORK_NAME="local_dbs_network"

#--------------------------------------------------------------------------
# Volume env
#--------------------------------------------------------------------------

# The database container data volume. | default: surrealdb_data
DB_VOLUME_DATA_NAME="${DB_CONTAINER_NAME}_data"
```

**Please note:** The SurrealDB Docker image doesn't offer additional environment 
variables.

##### **Start & Stop Docker container**

To start the SurrealDB container, you can run the following command:

```bash
# Navigate to SurrealDB database.
$ cd databases/surrealdb

# Run Docker Compose command.
$ docker compose up -d
```

To stop the SurrealDB container, you can run the following command:

```bash
$ docker compose down
```

##### **Connect to Database**

> **Note**
> : TablePlus currently doesn't support SurrealDB. You can use Postman to run 
queries on SurrealDB.

You will need to provide the following settings for your HTTP Request:

```ini
PORT="${DB_PORT}"

USER="${DB_USERNAME}"
PASSWORD="${DB_PASSWORD}"
```

Below is a cURL request using the settings:

```bash
curl --request POST \
	--header "Content-Type: application/json" \
	--header "NS: test" \
	--header "DB: test" \
	--user "${DB_USERNAME}:${DB_PASSWORD}" \
	--data "INFO FOR DB;" \
	http://localhost:${DB_PORT}/sql
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

</details>

---

<details>
<summary><a id="config-yugabytedb"><b>YugabyteDB</b></a></summary>

#### <ins>Configuring YugabyteDB</ins>

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
# Docker env
#--------------------------------------------------------------------------

# The project name. | default: yugabytedb
APP_NAME="yugabytedb"

#--------------------------------------------------------------------------
# Database (YugabyteDB) env
#--------------------------------------------------------------------------

# The YugabyteDB database container name. | default: yugabytedb
DB_CONTAINER_NAME="${APP_NAME}"

#--------------------------------------------------------------------------
# Network env
#--------------------------------------------------------------------------

# Map the database container PostgreSQL port to the host port. | default: 5433
DB_YSQL_PORT=5433

# Map the database container Cassandra port to the host port. | default: 9042
DB_YCQL_PORT=9042

# Map the database container Admin UI port to the host port. | default: 7001
DB_MASTER_PORT=7001

# Map the database container Admin UI port to the host port. | default: 9000
DB_TSERVER_PORT=9000

# The Docker network for the containers. | default: local_dbs_network
NETWORK_NAME="local_dbs_network"

#--------------------------------------------------------------------------
# Volume env
#--------------------------------------------------------------------------

# The database container data volume. | default: yugabytedb_data
DB_VOLUME_DATA_NAME="${DB_CONTAINER_NAME}_data"
```

**Please note:** The YugabyteDB Docker image doesn't offer additional 
environment variables. You are unable to create additional users via the 
YugabyteDB Docker image environment variables.

**Please note:** MacOS Monterey enables AirPlay receiving by default, which 
listens on port 7000. This conflicts with YugabyteDB default YB-Master port.

##### **Start & Stop Docker container**

To start the YugabyteDB container, you can run the following command:

```bash
# Navigate to YugabyteDB database.
$ cd databases/yugabytedb

# Run Docker Compose command.
$ docker compose up -d
```

To stop the YugabyteDB container, you can run the following command:

```bash
$ docker compose down
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
PORT="${DB_YSQL_PORT}"

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
PORT="${DB_YCQL_PORT}"

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

</details>

---

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
      - mongodb
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
