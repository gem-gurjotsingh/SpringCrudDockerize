# Spring Boot Application Dockerization

This is a basic Spring Boot application that writes records to a PostgreSQL database and reads them from a Redis cache in subsequent calls. We create three separate docker files for our application, database and cache and after that, we run all of them with containers.

## Installation

### Prerequisites

Before you begin, ensure you have met the following requirements:

- Java 8
- Maven 3.0+
- An IDE (IntelliJ is recommended)
- Docker Desktop installed on your system

### Clone Project

You can clone the project by using the following command:

<pre>
git clone https://github.com/gem-gurjotsingh/SpringCrudDockerize.git
</pre>

## How to run the containers?

Create docker images for all by running their respective docker files in the terminal:

<pre>
docker build -t postgresimage -f Dockerfile-postgres .

docker build -t redisimage -f Dockerfile-redis .

docker build -t springimage -f Dockerfile-spring .
</pre>

Now, create a docker network and run the containers within this network only. Run these commands in the terminal:

<pre>
docker network create spring-postgres-redis
  
docker run --name postgrescontainer --network spring-postgres-redis -d postgresimage
  
docker run --name rediscontainer --network spring-postgres-redis -d redisimage
  
docker run --name springcontainer --network spring-postgres-redis -d -p 8080:8080 springimage
</pre>

In the last step, we are mapping our host machine port 8080 to the docker port 8080. This is done so as to redirect all the API requests that are made to the host port into the Docker container of our Spring Boot application.
