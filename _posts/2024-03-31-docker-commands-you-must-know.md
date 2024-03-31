---
layout: post
title: 10 Docker Commands You Must Know
subtitle: A Guide to useful docker commands you must know
gh-repo: Abhishake63/abhishake-guides
gh-badge: [star, fork, follow]
tags: [docker, dev-ops]
author: Abhishake Sen Gupta
---

 Welcome to our guide on managing Docker images and containers! Whether you're new to Docker or looking to expand your knowledge, this comprehensive tutorial will equip you with the essential commands and techniques for handling Docker images and containers like a pro.

## Images

### Download an Image from a Registry Like DockerHub

```bash
docker pull image-name:tag
```

> Replace **`image-name`** with the name of the Docker image, and **`tag`** with the version or tag you want to pull.
>

### List All Images

```bash
docker images
```

### **Build an Image**

```bash
docker build -t custom-image-name:tag .
```

> Replace **`custom-image-name`** with the desired name for your image and **`tag`** with the version or tag.
>

### **Push an Image to a Registry**

```bash
docker push image-name:tag
```

> Replace **`image-name`** with the name of the Docker image and **`tag`** with the version or tag.
>

### **Remove an Image**

```bash
docker rmi image-name:tag
```

> Replace **`image-name`** with the name of the Docker image and **`tag`** with the version or tag. If you have running containers using the image, you may need to stop and remove them first.
>

### **Remove All Unused Images**

```bash
docker image prune
```

> This command removes all images that are not associated with any containers.
>

### Save Image to Tar File

```bash
docker save -o image-name.tar image-name:tag
```

### Load Image from Tar file

```bash
docker load -i image-name.tar
```

> Replace **`image-name`** with the name of the Docker image and **`tag`** with the version or tag.
>

## Containers

### **Run a Container**

```bash
docker run -d -p local-port:service-port image-name:tag
docker run -d --name my-container -p local-port:service-port image-name:tag
```

> Replace **`my-container`** with the desired container name, and **`image-name:tag`** with the Docker image and tag.
>

### **List Running Containers**

```bash
docker ps
docker ps -a
```

> Add the **`-a`** flag to include stopped containers
>

### **Start & Stop a Container**

```bash
docker stop container-id
docker start container-id
```

### Check Logs of a Container

```bash
docker logs container-id
```

### Removing Containers

```bash
docker rm container-id
docker rm container-id -f
docker rm -f $(docker ps -a -q)
```

> Add the **`-f`** flag to forcefully remove a running container

>**`docker rm -f $(docker ps -a -q)`**: Removes all containers, both running and stopped, by passing their IDs to the **`docker rm`** command.
>

**Here is the [Github Repo](https://github.com/Abhishake63/abhishake-guides) for this Article!**