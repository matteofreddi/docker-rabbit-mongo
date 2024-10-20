# docker-rabbit-mongo

A ready to use docker-compose configuration with rabbitmq,mongodb,mongo-express.

Containers:

- RabbitMQ
- MongoDB
- Mongo-Express

## Installation

```shell
git clone https://github.com/stdakov/docker-rabbit-mongo.git
cd docker-rabbit-mongo/
docker-compose up -d
```

## rabbitMQ

- http://localhost:15672/
- username: `admin`
- password: `admin`

## mongoDB

- username: `admin`
- password: `admin`  

## mongo-express

- http://localhost:8081/
- username: `admin`
- password: `pass`

#### Update container configuration

If you want to change some container configuration you need to rebuild the container:

```shell
docker-compose up -d --no-deps --build {service-name}
```

#### Connect to container:

```shell
docker-compose exec {service-name} bash
```

#### Restart everything:

```shell
docker-compose restart
```

#### Stop everything:

```shell
docker-compose stop
```

#### Remove everything:

```shell
docker-compose down -v
```

#### Reset:

##### rabbitMQ:
```shell
docker-compose exec rabbit sh -c "rabbitmqctl stop_app; rabbitmqctl reset; rabbitmqctl start_app"
```
or
```shell
docker exec rabbit sh -c "rabbitmqctl stop_app; rabbitmqctl reset; rabbitmqctl start_app"
```

##### mongoDB:
```shell
docker-compose exec mongo mongo --eval "\
Mongo().getDBNames().forEach(function(x) {\
  if (['admin', 'config', 'local'].indexOf(x) < 0) {\
    Mongo().getDB(x).dropDatabase();\
  }\
})\
"
```
or
```shell
docker exec -it mongo mongo ir --eval "\
Mongo().getDBNames().forEach(function(x) {\
  if (['admin', 'config', 'local'].indexOf(x) < 0) {\
    Mongo().getDB(x).dropDatabase();\
  }\
})\
"
```

## In you want to use docker without docker compose:

1. run rabbitMQ 
```shell
docker run --name rabbitmq --hostname rabbit  -p 5672:5672 -p 15672:15672 -e RABBITMQ_DEFAULT_USER=guest -e RABBITMQ_DEFAULT_PASS=guest -d rabbitmq:3-management
```
2. run mongodb
```shell
docker run --name mongodb -p 27017:27017 -d mongo:latest
```
3. remove rabbitmq:
```shell
docker ps -a
docker rm -f `<containerID>`
docker volume ls
docker volume rm `<volumeName>`
```
The docker system prune command will remove all stopped containers, all dangling images, and all unused networks:
```shell
docker system prune
```
You’ll be prompted to continue, use the -f or --force flag to bypass the prompt.
WARNING! This will remove:
        - all stopped containers
        - all networks not used by at least one container
        - all dangling images
        - all build cache
Are you sure you want to continue? [y/N]

If you also want to remove all unused volumes, pass the --volumes flag:

```shell
docker system prune --volumes
```
WARNING! This will remove:
        - all stopped containers
        - all networks not used by at least one container
        - all volumes not used by at least one container
        - all dangling images
        - all build cache
Are you sure you want to continue? [y/N] y
```shell
docker kill $(docker ps -q)
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
docker rm -v $(docker ps -a -q)
```
