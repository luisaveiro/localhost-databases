<h5 align="center">
  <a href="http://github.com/luisaveiro/localhost-databases" target="_blank">Localhost Databases</a>
</h5>

---

<p align="center">
  <img src="../../images/mongodb/logo-mongodb.svg" width="25%"/>
</p>

<h4 align="center">
  MongoDB is a NoSQL database.
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

[MongoDB](https://www.mongodb.com/) is a source-available cross-platform 
document-oriented database program. Classified as a NoSQL database program, 
MongoDB uses JSON-like documents with optional schemas.

You can learn about [MongoDB in 100 Seconds](https://youtu.be/-bt_y4Loofg) by 
watching [Fireship YouTube channel](https://www.youtube.com/@Fireship).

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

This repository utilizes [Docker](https://www.docker.com/) to run the MongoDB 
sample. So, before using the MongoDB, make sure you have Docker installed on 
your system.

## Download

To use MongoDB, you can clone the latest version of ***Localhost Databases*** 
repository for macOS, Linux and Windows.

```bash
# Clone this repository.
$ git clone git@github.com:luisaveiro/localhost-databases.git --branch main --single-branch
```

You can locate the MongoDB Docker configuration in the `databases` directory.

```bash
# Navigate to the MongoDB folder.
$ cd localhost-databases/databases/mongodb
```

## How To Use

There are a few steps you need to follow before you can have an MongoDB database 
set up and running in Docker container. I have outline the steps you would need 
to take to get started.

#### 1. **Environment Variables**

Before you start a database in a Docker container, you will need to create a 
DotEnv file. The DotEnv file will allow you to configure your database's 
credentials and map a container's port.

***Localhost Databases*** includes a `.env.example` file for MongoDB Database. You 
can run the following command in the terminal to create your DotEnv file.

```bash
# Navigate to a database.
$ cd databases/mongodb

# Create .env from .env.example.
$ cp .env.example .env
```

The MongoDB Docker Compose file uses the follow variables from the DotEnv 
file.

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

> [!NOTE]  
> You are unable to create additional users via the MongoDB 
Docker image environment variables. The Mongo username and password environment 
variables in the DotEnv are for the root user.

For a list of available environment variables that the MongoDB Docker image 
supports, you can visit [MongoDB Docker Hub](https://hub.docker.com/_/mongo) 
page.

#### 2. **Start Docker container**

To start the MongoDB container, you can run the following command:

```bash
# Navigate to MongoDB database.
$ cd databases/mongodb

# Run Docker Compose command.
$ docker compose up -d
```

##### Expected result

To check the MongoDB container is running and the port mapping is configured 
correctly, you can run the following command:

```bash
# List containers
$ docker ps  
```

You should see a similar output.

```bash
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS                            PORTS                      NAMES
71392ed17b0d   mongo:latest   "docker-entrypoint.s…"   10 seconds ago   Up 9 seconds (health: starting)   0.0.0.0:27017->27017/tcp   mongodb
```

#### 3. **Stop Docker container**

To stop the MongoDB container, you can run the following command:

```bash
$ docker compose down
```

#### 4. **Connect to Database**

To connect to your MongoDB container from your database client, you will 
need to provide the following settings:

```ini
URL=mongodb://${DB_USERNAME}:${DB_PASSWORD}@localhost:${DB_PORT}/
```

##### Expected result

Below is a screenshot of the settings used in TablePlus:

<p align="center">
  <a>
    <img 
      src="../../images/mongodb/tableplus-mongodb.png" 
      alt="TablePlus settings for MongoDB"
      width="50%">
  </a>
  <br>
  <sub><sup>TablePlus settings for MongoDB.</sup></sub>
</p>

---

<p align="center">
  <a href="http://github.com/luisaveiro" target="_blank">GitHub</a> •
  <a href="https://uk.linkedin.com/in/luisaveiro" target="_blank">LinkedIn</a> •
  <a href="https://twitter.com/luisdeaveiro" target="_blank">Twitter</a>
</p>
