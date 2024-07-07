# Intro

## Docker Desktop Installation
- download and install [docker desktop](https://www.docker.com/products/docker-desktop/)
- create an account in [docker hub](https://hub.docker.com/signup)
- verify installation by opening terminal and running
```
docker run --rm hello-world
```
- remove the testing image
```
docker rmi hello-world:latest
```

## Postgres

### Container Interaction

#### Show Running Containers
```
docker ps
```

#### Show All Containers
```
docker ps -a
```

#### Create a container
```
docker create --name my-postgres -e POSTGRES_USER=root -e POSTGRES_PASSWORD=pass -p 5432:5432 postgres:16
```

#### Start a container
```
docker start my-postgres
```

#### Stop a container
```
docker stop my-postgres
```

#### Connect to the container
```
docker exec -it my-postgres bash
```

#### Exit from the container
```
exit
```

#### Delete a container
```
docker rm my-postgres
```
_By deleting a container all its data will get lost._

### Database Interaction

#### Connect to the database engine
```
psql -U root
```

#### Basic Commands
- Show databases `\l`
- Create a database `create database demo;`
- Connect to a database `\c demo`
- Drop a database `drop database demo;` _not in prodcuction if possible_
- Exit from database `exit` or `\q`

### Connect with Beekeeper
- Make sure that the container is started
- Connect with the URL `postgres://root:pass@localhost:5432/demo`
