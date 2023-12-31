<h5 align="center">
  <a href="http://github.com/luisaveiro/localhost-databases" target="_blank">Localhost Databases</a>
</h5>

---

<p align="center">
  <img src="../../images/redis/logo-redis.svg" width="20%"/>
</p>

<h4 align="center">
  Redis is an in-memory data structure store.
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

[Redis](https://redis.io/) is an in-memory data structure store, used as a 
distributed, in-memory key–value database, cache and message broker, with 
optional durability.

You can learn about [Redis in 100 Seconds](https://youtu.be/G1rOthIU-uo) by 
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

This repository utilizes [Docker](https://www.docker.com/) to run the Redis 
sample. So, before using the Redis, make sure you have Docker installed on 
your system.

## Download

To use Redis, you can clone the latest version of ***Localhost Databases*** 
repository for macOS, Linux and Windows.

```bash
# Clone this repository.
$ git clone git@github.com:luisaveiro/localhost-databases.git --branch main --single-branch
```

You can locate the Redis Docker configuration in the `databases` directory.

```bash
# Navigate to the Redis folder.
$ cd localhost-databases/databases/redis
```

## How To Use

There are a few steps you need to follow before you can have an Redis database 
set up and running in Docker container. I have outline the steps you would need 
to take to get started.

#### 1. **Environment Variables**

Before you start a database in a Docker container, you will need to create a 
DotEnv file. The DotEnv file will allow you to configure your database's 
credentials and map a container's port.

***Localhost Databases*** includes a `.env.example` file for Redis Database. You 
can run the following command in the terminal to create your DotEnv file.

```bash
# Navigate to a database.
$ cd databases/redis

# Create .env from .env.example.
$ cp .env.example .env
```

The Redis Docker Compose file uses the follow variables from the DotEnv 
file.

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

> [!NOTE]    
> The Redis Docker image doesn't offer additional environment variables.

#### 2. **Start Docker container**

To start the Redis container, you can run the following command:

```bash
# Navigate to Redis database.
$ cd databases/redis

# Run Docker Compose command.
$ docker compose up -d
```

##### Expected result

To check the Redis container is running and the port mapping is configured 
correctly, you can run the following command:

```bash
# List containers
$ docker ps  
```

You should see a similar output.

```bash
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS                            PORTS                    NAMES
cd1bd4fb200d   redis:alpine   "docker-entrypoint.s…"   9 seconds ago   Up 8 seconds (health: starting)   0.0.0.0:6379->6379/tcp   redis_db
```

#### 3. **Stop Docker container**

To stop the Redis container, you can run the following command:

```bash
$ docker compose down
```

#### 4. **Connect to Database**

To connect to your Redis container from your database client, you will need to 
provide the following settings:

```ini
HOST=127.0.0.1
PORT="${DB_PORT}"
```

##### Expected result

Below is a screenshot of the settings used in TablePlus:

<p align="center">
  <a>
    <img 
      src="../../images/redis/tableplus-redis.png" 
      alt="TablePlus settings for Redis"
      width="50%">
  </a>
  <br>
  <sub><sup>TablePlus settings for Redis.</sup></sub>
</p>

---

<p align="center">
  <a href="http://github.com/luisaveiro" target="_blank">GitHub</a> •
  <a href="https://uk.linkedin.com/in/luisaveiro" target="_blank">LinkedIn</a> •
  <a href="https://twitter.com/luisdeaveiro" target="_blank">Twitter</a>
</p>
