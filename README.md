# docker-cheat-sheet
A bunch of Docker commands and alias to make my life easier     

## Useful Aliases  
How to add useful aliases to your terminal:
1- download this dotfile inside your home directory: [.docker_aliases](.docker_aliases)
2- add this config in your .zshrc file:

	# Docker aliases
	if [ -f ~/.docker_aliases ]; then
	  source ~/.docker_aliases
	fi

NOTE: If you are using `Oh My ZSH` docker plugin, disable it in order to avoid conflicts.

## Commands
**See running container**
> docker ps -a

**Display container logs**
> docker logs -f CONTAINER_NAME 

**Inspect a container**
> docker inspect CONTAINER_NAME

**SSH into a running container**
> docker exec -it CONTAINER_NAME bash

**Stop a running container**
> docker stop HASH_ID

**Force shutdown of the specified container**
> docker kill HASH_ID

**Remove the specified container from this machine**
> docker rm HASH_ID

**Remove all containers from this machine**
> docker rm $(docker ps -a -q)

**List Docker volume**
> docker volume ls

# Cleanup & Common issues

**Prune all stopped containers**
> docker container prune -f

**Prune volumes**
> docker volume prune

**Prune networks**
> docker network prune -f

**Prune unused images**
This will only prune images not referenced by any existing containers
> docker image prune -a

**Common error removing network with**
ERROR: error while removing network: network…

    > docker network ls
    # find container associated to that network
    > docker network inspect my_network 
    # remove zombie container
    > docker stop local-dev_monolith-bus-adapter_1
    > docker rm local-dev_monolith-bus-adapter_1

NOTE: IF NOTHING WORKS, RESTART DOCKER DAEMON. THEN: 
> docker network prune -f

**Remove everithing and build cache**
WARNING: This will remove all your cached images, networks, etc. 
> docker system prune -a

## Docker Compose
> docker-compose -f common.yml up -d --remove-orphans

Stop services only
> docker-compose stop

Stop and remove containers, networks..
> docker-compose -f common.yml down 

Down and remove volumes
> docker-compose -f common.yml down --volumes 

Down and remove images
> docker-compose -f common.yml down --rmi <all|local> 

## Some common services containerized
Run RabbitMQ locally
> docker run -d -p 15672:15672 -p 5672:5672 -p 5671:5671 --hostname rabbitmq --name rabbitmq rabbitmq:3-management

- Login at http://localhost:15672/ using guest/guest
- Verify rabbit connection in port:
-      nc -vz localhost 5672

Run Redis locally
> docker run -d --name redis -p 6379:6379 redis

connect to redis using `redis-cli -h localhost -p 6379`

Run MySQL locally
> docker run -p 3306:3306 --name some-mysql -e MYSQL_ROOT_PASSWORD=my_password -d mysql

connect to mysql running in the container
> docker run -it --rm mysql mysql -hsome-mysql -uexample-user -p

To restart services when your machine restart just add this flag:

> --restart always

## Portainer (Docker UI)
> docker volume create portainer_data
> docker run -d -p 8000:8000 -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer

Access portainer from http://localhost:9000
