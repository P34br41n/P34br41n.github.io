---
layout: posts
title: Docker
---

# What is this about?
The Whale thingy that allows you to tape things together and run them without having to figure out how they independently work!

## The stuff you'll use all the time
I'm not going to go over the whole docker manual here, just some stuff I use daily and need somewhere to copy paste from. Think of this as a cheatsheet basically ;)

Stop all containers
```
docker stop $(docker ps -a -q)
```
Remove all containers
```
docker rm $(docker ps -a -q)
```
Remove all images
```
docker rmi $(docker images -q)
```
Remove stopped containers
```
docker container prune
```
Remove stopped containers, unused networks, dangling images, build cache
```
docker system prune -a
```
Run a bash in a container
```
docker exec -it <container_id_or_name> /bin/sh
```

## Docker-compose 
So basically you have your 30 lines of bash and 20 different Dockerfiles to run services together and keep messing things up? Just build the builder and let it build it auto! :p

I'm not going to copy paste some tutorial and add flowers to it, just go [here](https://docs.docker.com/compose/gettingstarted/) and RTFM...