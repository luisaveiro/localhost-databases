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
    Database Docker containers for local development
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

- Docker Engine >= 19.03.0

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
the database's Docker Compose file to the `docker-compose` command by 
using the `-f` flag. 

```bash
docker-compose -f docker-compose.database-name.yml up -d
```

An example of the `docker-compose` command would be as follows:

```bash
docker-compose -f docker-compose.redis.yml up -d
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

The following databases are part of this repository's collection:

- [MariaDB](#config-mariadb)
- [MongoDB](#config-mongodb)
- [Microsoft SQL Server (MSSQL)](#config-mssql)
- [MySQL](#config-mysql)
- [PostgreSQL](#config-postgres)
- [Redis](#config-redis)


Below I have provided more information on how to configure each database, 
start the database container and connect to the database via 
[TablePlus](https://tableplus.com/) app.

---

#### <a id="config-mariadb"></a> <ins>Configuring MariaDB</ins>

[MariaDB](https://mariadb.org/) Server is one of the most popular open source 
relational databases. It's made by the original developers of MySQL.

##### **Environment Variables**

MariaDB Docker Compose file uses the follow variables from the DotEnv file.

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

For a list of available environment variables that MariaDB Docker image 
supports, you can visit [MariaDB Docker Hub](https://hub.docker.com/_/mariadb) 
page.

##### **Start & Stop Docker container**

To start MariaDB container, you can run the following command:

```bash
docker-compose -f docker-compose.mariadb.yml up -d
```

To stop MariaDB container, you can run the following command:

```bash
docker-compose -f docker-compose.mariadb.yml down
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

MongoDB Docker Compose file uses the follow variables from the DotEnv file.

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

For a list of available environment variables that MongoDB Docker image 
supports, you can visit [MongoDB Docker Hub](https://hub.docker.com/_/mongo) 
page.

##### **Start & Stop Docker container**

To start MongoDB container, you can run the following command:

```bash
docker-compose -f docker-compose.mongodb.yml up -d
```

To stop MongoDB container, you can run the following command:

```bash
docker-compose -f docker-compose.mongodb.yml down
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
> : MSSQL is not support on Apple Silicon. There is an open [GitHub issue](https://github.com/microsoft/mssql-docker/issues/734).

##### **DotEnv Variables**

MSSQL Docker Compose file uses the follow variables from the DotEnv file.

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

For a list of available environment variables that MSSQL Docker image 
supports, you can visit [SQL Docs](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-configure-environment-variables?view=sql-server-ver15) 
page.

##### **Start & Stop Docker container**

To start MSSQL container, you can run the following command:

```bash
docker-compose -f docker-compose.mssql.yml up -d
```

To stop MSSQL container, you can run the following command:

```bash
docker-compose -f docker-compose.mssql.yml down
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

MySQL Docker Compose file uses the follow variables from the DotEnv file.

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

For a list of available environment variables that MySQL Docker image 
supports, you can visit [MySQL Docker Hub](https://hub.docker.com/_/mysql) 
page.

##### **Start & Stop Docker container**

To start MySQL container, you can run the following command:

```bash
docker-compose -f docker-compose.mysql.yml up -d
```

To stop MySQL container, you can run the following command:

```bash
docker-compose -f docker-compose.mysql.yml down
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

PostgreSQL Docker Compose file uses the follow variables from the DotEnv file.

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

For a list of available environment variables that PostgreSQL Docker image 
supports, you can visit [PostgreSQL Docker Hub](https://hub.docker.com/_/postgres) 
page.

##### **Start & Stop Docker container**

To start PostgreSQL container, you can run the following command:

```bash
docker-compose -f docker-compose.pgsql.yml up -d
```

To stop PostgreSQL container, you can run the following command:

```bash
docker-compose -f docker-compose.pgsql.yml down
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

Redis Docker Compose file uses the follow variables from the DotEnv file.

```ini
#--------------------------------------------------------------------------
# Redis env
#--------------------------------------------------------------------------

REDIS_CONTAINER_NAME="${APP_NAME}_redis"

REDIS_PORT=6379
```

**Please note:** Redis Docker image doesn't offer additional environment variables.

##### **Start & Stop Docker container**

To start Redis container, you can run the following command:

```bash
docker-compose -f docker-compose.redis.yml up -d
```

To stop Redis container, you can run the following command:

```bash
docker-compose -f docker-compose.redis.yml down
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

## Docker Network

If you wish to attach Docker containers to the database network to allow other 
containers to access your database. I have outlined the necessary 
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

[TablePlus](https://tableplus.com/) is a modern, native tool for database 
management that supports whole set of relational databases (and some NoSQL).

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
