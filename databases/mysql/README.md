<h5 align="center">
  <a href="http://github.com/luisaveiro/localhost-databases" target="_blank">Localhost Databases</a>
</h5>

---

<p align="center">
  <img src="../../images/mysql/logo-mysql.svg" width="15%"/>
</p>

<h4 align="center">
  MySQL is the world's most popular relational database.
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

[MySQL](https://www.mysql.com/) is the world's most popular open-source 
relational database management system (RDBMS).

## Disclaimer

> [!IMPORTANT]  
> ***Localhost Databases*** is not affiliated with the databases' 
developers/owners and is not an official product.

***Localhost Databases*** has been developed to run databases in a local 
Docker environment. To install a production instance, read the databases' 
respective installation guides.

## Getting Started

You will need to make sure your system meets the following prerequisites:

- Docker Engine >= 20.10.0

This repository utilizes [Docker](https://www.docker.com/) to run the MySQL 
sample. So, before using the MySQL, make sure you have Docker installed on 
your system.

## Download

To use MySQL, you can clone the latest version of ***Localhost Databases*** 
repository for macOS, Linux and Windows.

```bash
# Clone this repository.
$ git clone git@github.com:luisaveiro/localhost-databases.git --branch main --single-branch
```

You can locate the MySQL Docker configuration in the `databases` directory.

```bash
# Navigate to the MySQL folder.
$ cd localhost-databases/databases/mysql
```

## How To Use

There are a few steps you need to follow before you can have an MySQL database 
set up and running in Docker container. I have outline the steps you would need 
to take to get started.

#### 1. **Environment Variables**

Before you start a database in a Docker container, you will need to create a 
DotEnv file. The DotEnv file will allow you to configure your database's 
credentials and map a container's port.

***Localhost Databases*** includes a `.env.example` file for MySQL Database. You 
can run the following command in the terminal to create your DotEnv file.

```bash
# Navigate to a database.
$ cd databases/mysql

# Create .env from .env.example.
$ cp .env.example .env
```

The MySQL Docker Compose file uses the follow variables from the DotEnv 
file.

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

> [!NOTE]   
> MySQL allows root's password to be empty.

For a list of available environment variables that the MySQL Docker image 
supports, you can visit [MySQL Docker Hub](https://hub.docker.com/_/mysql) 
page.

#### 2. **Start Docker container**

To start the MySQL container, you can run the following command:

```bash
# Navigate to MySQL database.
$ cd databases/mysql

# Run Docker Compose command.
$ docker compose up -d
```

##### Expected result

To check the MySQL container is running and the port mapping is configured 
correctly, you can run the following command:

```bash
# List containers
$ docker ps  
```

You should see a similar output.

```bash
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS                             PORTS                               NAMES
cb9bac3a9026   mysql:latest   "docker-entrypoint.s…"   11 seconds ago   Up 10 seconds (health: starting)   0.0.0.0:3306->3306/tcp, 33060/tcp   mysql_db
```

#### 3. **Stop Docker container**

To stop the MySQL container, you can run the following command:

```bash
$ docker compose down
```

#### 4. **Connect to Database**

To connect to your MySQL container from your database client, you will 
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
      src="../../images/mysql/tableplus-mysql.png" 
      alt="TablePlus settings for MySQL"
      width="50%">
  </a>
  <br>
  <sub><sup>TablePlus settings for MySQL.</sup></sub>
</p>

---

<p align="center">
  <a href="http://github.com/luisaveiro" target="_blank">GitHub</a> •
  <a href="https://uk.linkedin.com/in/luisaveiro" target="_blank">LinkedIn</a> •
  <a href="https://twitter.com/luisdeaveiro" target="_blank">Twitter</a>
</p>
