---
layout: post
title: How to Scale a Spring Boot Application
subtitle: A Guide to Efficient Deployment with Docker and Nginx Load Balancer
gh-repo: Abhishake63/spring-boot-nginx
gh-badge: [star, fork, follow]
tags: [spring-boot, nginx]
author: Abhishake Sen Gupta
---

## Create a Simple Spring Boot Project

Go to [Spring Initializr](https://start.spring.io/)  & initialize a project with just one dependency `spring web` with packaging `war`

### Controller

```java
package com.example.nginx.controller;

import java.net.InetAddress;
import java.net.UnknownHostException;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/")
    public String helloWorld() {
        return "Hello World";
    }

    @GetMapping("/check")
    public String check() {
        try {
            return "Container ID: " + InetAddress.getLocalHost().getHostName();
        } catch (UnknownHostException e) {
            e.printStackTrace();
        }
        return "checking failed";
    }
}
```

## Dockerizing the Project

### Dockerfile

```docker
FROM eclipse-temurin:17-alpine
RUN mkdir /opt/app
COPY target/*.war /opt/app/app.war
ENTRYPOINT ["java", "-jar", "/opt/app/app.war"]
```

### Build the WAR file

```bash
mvn clean package
```

### Build the Docker image

```bash
docker build -t nginx .
```

Go to the project directory and run these above commands

### Run the Docker container

```bash
docker run -d -p 1111:8080 nginx:latest
docker run -d -p 2222:8080 nginx:latest
docker run -d -p 3333:8080 nginx:latest
```

Run 3 containers with the Docker image


## Editing Nginx Config

```java
http {
	include mime.types;

	upstream backendserver {
		server localhost:1111;
		server localhost:2222;
		server localhost:3333;
	}

	server {
		listen 8080;

		location / {
			proxy_pass http://backendserver/;
		}
	}
}

events {}
```

Go to `/etc/nginx` & edit `nginx.conf` file with the above config & restart `nginx`

## Check if Nginx Load Balancer is Working

```bash
curl http://localhost:8080/check
```

Check if the response contains different container identifiers.

By combining these steps, you should be able to determine whether `Nginx` is successfully `round-robin distributing requests` among your Spring Boot containers.