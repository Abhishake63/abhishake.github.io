---
layout: post
title: How to Scale a Spring Boot Application
subtitle: A Guide to Efficient Deployment with Docker and Nginx Load Balancer
gh-repo: Abhishake63/spring-boot-nginx
gh-badge: [star, fork, follow]
tags: [spring-boot, java, nginx]
author: Abhishake Sen Gupta
---

Welcome to the Spring Boot "Nginx" project! This is a simple project that demonstrates the way of scaling a Spring Boot Application using Nginx as a Load Balancer.

## 1. Overview

This project serves as a starting point for understanding how to scale a basic Spring Boot application using Docker for Containerization and Nginx for Load Balancing. It includes the necessary setup and dependencies to quickly get you up and running.

## 2. Prerequisites

Before you begin, ensure you have met the following requirements:

- Java Development Kit (JDK) installed (version 17 or higher)
- Maven build tool installed
- Nginx, Docker installed****

## 3. Maven Dependencies

> First, we need to add the dependencies to our `pom.xml.`

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

## 4. The Controller

```java
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

## 5. Dockerizing the Project

### 5.1. Dockerfile

```docker
FROM eclipse-temurin:17-alpine
RUN mkdir /opt/app
COPY target/*.war /opt/app/app.war
ENTRYPOINT ["java", "-jar", "/opt/app/app.war"]
```

### 5.2. Build the WAR file

```bash
mvn clean package
```

### 5.3. Build the Docker image

```bash
docker build -t nginx .
```

> Go to the project directory and run these above commands

### 5.4. Run the Docker container

```bash
docker run -d -p 1111:8080 nginx:latest
docker run -d -p 2222:8080 nginx:latest
docker run -d -p 3333:8080 nginx:latest
```

> Run 3 containers with the Docker image

## 6. Editing Nginx Config

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

> Go to `/etc/nginx` & edit `nginx.conf` file with the above config & restart `nginx`

## 7. Check if Nginx Load Balancer is Working

```bash
curl http://localhost:8080/check
```

> Check if the response contains different container identifiers.

> By combining these steps, you should be able to determine whether `Nginx` is successfully `round-robin distributing requests` among your Spring Boot containers.