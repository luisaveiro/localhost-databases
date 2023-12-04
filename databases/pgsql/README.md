<h5 align="center">
  <a href="http://github.com/luisaveiro/localhost-databases" target="_blank">Localhost Databases</a>
</h5>

---

<p align="center">
  <img src="../../images/pgsql/logo-pgsql.svg" width="15%"/>
</p>

<h4 align="center">
  PostgreSQL also known as Postgres.
</h4>

<p align="center">
  <a href="#about">About</a> •
  <a href="#disclaimer">Disclaimer</a> •
  <a href="#getting-started">Getting Started</a> •
  <a href="#download">Download</a> •
  <a href="#how-to-use">How To Use</a>
</p>

---

## About

[PostgreSQL](https://www.postgresql.org/), also known as Postgres, is a free 
and open-source relational database management system emphasizing extensibility 
and SQL compliance.

## Disclaimer

> **Note**  
> ***Localhost Databases*** is not affiliated with the databases' 
developers/owners and is not an official product.

***Localhost Databases*** has been developed to run databases in a local 
Docker environment. To install a production instance, read the databases' 
respective installation guides.

## Getting Started

You will need to make sure your system meets the following prerequisites:

- Docker Engine >= 20.10.0

This repository utilizes [Docker](https://www.docker.com/) to run the PostgreSQL 
sample. So, before using the PostgreSQL, make sure you have Docker installed on 
your system.

## Download

To use PostgreSQL, you can clone the latest version of ***Localhost Databases*** 
repository for macOS, Linux and Windows.

```bash
# Clone this repository.
$ git clone git@github.com:luisaveiro/localhost-databases.git --branch main --single-branch
```

You can locate the PostgreSQL Docker configuration in the `databases` directory.

```bash
# Navigate to the PostgreSQL folder.
$ cd localhost-databases/databases/pgsql
```

## How To Use

There are a few steps you need to follow before you can have an PostgreSQL database 
set up and running in Docker container. I have outline the steps you would need 
to take to get started.

#### 1. **Environment Variables**

Before you start a database in a Docker container, you will need to create a 
DotEnv file. The DotEnv file will allow you to configure your database's 
credentials and map a container's port.

***Localhost Databases*** includes a `.env.example` file for PostgreSQL Database. You 
can run the following command in the terminal to create your DotEnv file.

```bash
# Navigate to a database.
$ cd databases/pgsql

# Create .env from .env.example.
$ cp .env.example .env
```

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

#### 2. **Start Docker container**

To start the PostgreSQL container, you can run the following command:

```bash
# Navigate to PostgreSQL database.
$ cd databases/pgsql

# Run Docker Compose command.
$ docker compose up -d
```

##### Expected result

To check the PostgreSQL container is running and the port mapping is configured 
correctly, you can run the following command:

```bash
# List containers
$ docker ps  
```

You should see a similar output.

```bash
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS                    PORTS                    NAMES
4f4213061f5c   postgres:latest   "docker-entrypoint.s…"   43 seconds ago   Up 42 seconds (healthy)   0.0.0.0:5432->5432/tcp   pgsql_db
```

#### 3. **Stop Docker container**

To stop the PostgreSQL container, you can run the following command:

```bash
$ docker compose down
```

#### 4. **Connect to Database**

To connect to your PostgreSQL container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${DB_PORT}"

USER="${DB_USERNAME}"
PASSWORD="${DB_PASSWORD}"

DATABASE="${DB_DATABASE}"
```

##### Expected result

Below is a screenshot of the settings used in TablePlus:

<p align="center">
  <a>
    <img 
      src="../../images/pgsql/tableplus-pgsql.png" 
      alt="TablePlus settings for PostgreSQL"
      width="50%">
  </a>
  <br>
  <sub><sup>TablePlus settings for PostgreSQL.</sup></sub>
</p>

---

<p align="center">
  <a href="http://github.com/luisaveiro" target="_blank">GitHub</a> •
  <a href="https://uk.linkedin.com/in/luisaveiro" target="_blank">LinkedIn</a> •
  <a href="https://twitter.com/luisdeaveiro" target="_blank">Twitter</a>
</p>
