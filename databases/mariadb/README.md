<h5 align="center">
  <a href="http://github.com/luisaveiro/localhost-databases" target="_blank">Localhost Databases</a>
</h5>

---

<p align="center">
  <img src="../../images/mariadb/logo-mariadb.svg" width="15%"/>
</p>

<h4 align="center">
  MariaDB Server is one of the most popular open source relational databases.
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

[MariaDB](https://mariadb.org/) Server is one of the most popular open source 
relational databases. It's made by the original developers of MySQL.

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

This repository utilizes [Docker](https://www.docker.com/) to run the MariaDB 
sample. So, before using the MariaDB, make sure you have Docker installed on 
your system.

## Download

To use MariaDB, you can clone the latest version of ***Localhost Databases*** 
repository for macOS, Linux and Windows.

```bash
# Clone this repository.
$ git clone git@github.com:luisaveiro/localhost-databases.git --branch main --single-branch
```

You can locate the MariaDB Docker configuration in the `databases` directory.

```bash
# Navigate to the MariaDB folder.
$ cd localhost-databases/databases/mariadb
```

## How To Use

There are a few steps you need to follow before you can have an MariaDB database 
set up and running in Docker container. I have outline the steps you would need 
to take to get started.

#### 1. **Environment Variables**

Before you start a database in a Docker container, you will need to create a 
DotEnv file. The DotEnv file will allow you to configure your database's 
credentials and map a container's port.

***Localhost Databases*** includes a `.env.example` file for MariaDB Database. You 
can run the following command in the terminal to create your DotEnv file.

```bash
# Navigate to a database.
$ cd databases/mariadb

# Create .env from .env.example.
$ cp .env.example .env
```

The MariaDB Docker Compose file uses the follow variables from the DotEnv 
file.

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

> **Note**  
> MariaDB allows root's password to be empty.

For a list of available environment variables that the MariaDB Docker image 
supports, you can visit [MariaDB Docker Hub](https://hub.docker.com/_/mariadb) 
page.

#### 2. **Start Docker container**

To start the MariaDB container, you can run the following command:

```bash
# Navigate to MariaDB database.
$ cd databases/mariadb

# Run Docker Compose command.
$ docker compose up -d
```

##### Expected result

To check the MariaDB container is running and the port mapping is configured 
correctly, you can run the following command:

```bash
# List containers
$ docker ps  
```

You should see a similar output.

```bash
CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS                            PORTS                    NAMES
506cefa9fc77   mariadb:latest   "docker-entrypoint.s…"   5 seconds ago   Up 3 seconds (health: starting)   0.0.0.0:3306->3306/tcp   mariadb
```

#### 3. **Stop Docker container**

To stop the MariaDB container, you can run the following command:

```bash
$ docker compose down
```

#### 4. **Connect to Database**

To connect to your MariaDB container from your database client, you will 
need to provide the following settings:

```ini
HOST=127.0.0.1
PORT="${DB_PORT}"

USER="${DB_USERNAME}"
PASSWORD="${DB_PASSWORD}"
```

##### Expected result

Below is a screenshot of the settings used in TablePlus:

<p align="center">
  <a>
    <img 
      src="../../images/mariadb/tableplus-mariadb.png" 
      alt="TablePlus settings for MariaDB"
      width="50%">
  </a>
  <br>
  <sub><sup>TablePlus settings for MariaDB.</sup></sub>
</p>

---

<p align="center">
  <a href="http://github.com/luisaveiro" target="_blank">GitHub</a> •
  <a href="https://uk.linkedin.com/in/luisaveiro" target="_blank">LinkedIn</a> •
  <a href="https://twitter.com/luisdeaveiro" target="_blank">Twitter</a>
</p>
