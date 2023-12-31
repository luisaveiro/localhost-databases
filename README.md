<p align="center">
  <a href="https://supportukrainenow.org" target="_blank">
    <img src="./images/standwithukraine.png" alt="#StandWithUkraine" width="100%">
  </a>
</p>
<br/>
<p align="center">
  <a href="http://github.com/luisaveiro/localhost-databases">
    <img src="./images/databases.svg" alt="databases" width="50%">
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

> [!IMPORTANT]  
> ***Localhost Databases*** is not affiliated with the databases' 
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

Localhost Databases include 14 database servers. The following databases are 
part of this repository's collection:

- [Cassandra](databases/cassandra/README.md)
- [CockroachDB](databases/cockroachdb/README.md)
- [CouchDB](databases/couchdb/README.md)
- [DynamoDB Local](databases/dynamodb/README.md)
- [EdgeDB](databases/edgedb/README.md)
- [MariaDB](databases/mariadb/README.md)
- [MongoDB](databases/mongodb/README.md)
- [Microsoft SQL Server (MSSQL)](databases/mssql/README.md)
- [MySQL](databases/mysql/README.md)
- [Neo4j](databases/neo4j/README.md)
- [PostgreSQL](databases/pgsql/README.md)
- [Redis](databases/redis/README.md)
- [SurrealDB](databases/surrealdb/README.md)
- [YugabyteDB](databases/yugabytedb/README.md)

I have provided a README file for each database on how to configure the database, 
start the database container and connect to the database via a database client 
app.

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
