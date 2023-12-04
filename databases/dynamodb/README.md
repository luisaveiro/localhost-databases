<h5 align="center">
  <a href="http://github.com/luisaveiro/localhost-databases" target="_blank">Localhost Databases</a>
</h5>

---

<p align="center">
  <img src="../../images/dynamodb/logo-dynamodb.svg" width="10%"/>
</p>

<h4 align="center">
  Amazon DynamoDB is a fully managed NoSQL database service.
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

[Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.html) 
is a fully managed NoSQL database service that provides fast and predictable 
performance with seamless scalability. DynamoDB local is a downloadable version 
of Amazon DynamoDB, you can develop and test applications without accessing the 
DynamoDB web service.

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

This repository utilizes [Docker](https://www.docker.com/) to run the DynamoDB 
sample. So, before using the DynamoDB, make sure you have Docker installed on 
your system.

## Download

To use DynamoDB, you can clone the latest version of ***Localhost Databases*** 
repository for macOS, Linux and Windows.

```bash
# Clone this repository.
$ git clone git@github.com:luisaveiro/localhost-databases.git --branch main --single-branch
```

You can locate the DynamoDB Docker configuration in the `databases` directory.

```bash
# Navigate to the DynamoDB folder.
$ cd localhost-databases/databases/dynamodb
```

## How To Use

There are a few steps you need to follow before you can have an DynamoDB database 
set up and running in Docker container. I have outline the steps you would need 
to take to get started.

#### 1. **Environment Variables**

Before you start a database in a Docker container, you will need to create a 
DotEnv file. The DotEnv file will allow you to configure your database's 
credentials and map a container's port.

***Localhost Databases*** includes a `.env.example` file for DynamoDB Database. You 
can run the following command in the terminal to create your DotEnv file.

```bash
# Navigate to a database.
$ cd databases/dynamodb

# Create .env from .env.example.
$ cp .env.example .env
```

The DynamoDB Docker Compose file uses the follow variables from the DotEnv 
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

#### 2. **Start Docker container**

To start the DynamoDB Local container, you can run the following command:

```bash
# Navigate to DynamoDB database.
$ cd databases/dynamodb

# Run Docker Compose command.
$ docker compose up -d
```

##### Expected result

To check the DynamoDB container is running and the port mapping is configured 
correctly, you can run the following command:

```bash
# List containers
$ docker ps  
```

You should see a similar output.

```bash
CONTAINER ID   IMAGE                          COMMAND                  CREATED         STATUS              PORTS                    NAMES
54800a208fc1   amazon/dynamodb-local:latest   "java -jar DynamoDBL…"   2 minutes ago   Up About a minute   0.0.0.0:8000->8000/tcp   dynamodb
```

#### 3. **Stop Docker container**

To stop the DynamoDB container, you can run the following command:

```bash
$ docker compose down
```

#### 4. **Connect to Database**

> **Note**  
> TablePlus currently doesn't support DynamoDB. You can use NoSQL Workbench.

To connect to your DynamoDB container from your database client, you will 
need to provide the following settings:

```ini
HOST=localhost
PORT="${DB_PORT}"
```

##### Expected result

Below is a screenshot of the settings used in NoSQL Workbench:

<p align="center">
  <a>
    <img 
      src="../../images/dynamodb/nosql-workbench-dynamodb.png" 
      alt="NoSQL Workbench settings for DynamoDB Local"
      width="50%">
  </a>
  <br>
  <sub><sup>NoSQL Workbench settings for DynamoDB Local.</sup></sub>
</p>

---

<p align="center">
  <a href="http://github.com/luisaveiro" target="_blank">GitHub</a> •
  <a href="https://uk.linkedin.com/in/luisaveiro" target="_blank">LinkedIn</a> •
  <a href="https://twitter.com/luisdeaveiro" target="_blank">Twitter</a>
</p>
