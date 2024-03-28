---
layout: post
title: Error Handling with Spring Boot
subtitle: A Comprehensive Guide to Handle Errors Gracefully with Spring Boot
gh-repo: Abhishake63/spring-boot-errorhandling
gh-badge: [star, fork, follow]
tags: [spring-boot, java]
author: Abhishake Sen Gupta
---

Welcome to our guide on implementing robust error handling in a Spring Boot application. We'll leverage Spring Boot's capabilities to create a system that effectively captures, processes, and presents errors.

## 1. Overview

This guide outlines the implementation of a robust error handling system in a Spring Boot application. By leveraging Spring Boot's features, we'll create mechanisms to capture, process, and present errors effectively.

## 2. Prerequisites

Before we dive into the implementation, make sure you have the necessary prerequisites installed on your system:

- **Java Development Kit (JDK)**: Ensure you have JDK version 17 or higher installed.
- **Maven**: You'll need Maven as a build tool for managing dependencies and building the project.

## 3. Maven Dependencies

> Add the required dependencies to your `pom.xml`:
>

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-devtools</artifactId>
  <scope>runtime</scope>
  <optional>true</optional>
</dependency>
<dependency>
  <groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
  <optional>true</optional>
</dependency>
```

## 4. Configure Exception Handler

### 4.1. ErrorResponse

```java
@Getter
@Setter
@AllArgsConstructor
public class ErrorResponse {
    private String error;
    private String message;
    private int errorCode;
}
```

### 4.2. GlobalExceptionHandler

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    private static final Logger logger = LoggerFactory.getLogger(GlobalExceptionHandler.class);

    @ExceptionHandler(IllegalArgumentException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    public ErrorResponse handleIllegalArgumentRequestException(IllegalArgumentException ex) {
        logger.error("Call handleIllegalArgumentRequestException: {}", ex.getMessage());
        return new ErrorResponse(HttpStatus.BAD_REQUEST.getReasonPhrase(), ex.getMessage(),
                HttpStatus.BAD_REQUEST.value());
    }

    @ExceptionHandler(Exception.class)
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    public ErrorResponse handleGenericException(Exception ex) {
        logger.error("Call handleGenericException: {}", ex.getMessage());
        return new ErrorResponse(HttpStatus.INTERNAL_SERVER_ERROR.getReasonPhrase(), ex.getMessage(),
                HttpStatus.INTERNAL_SERVER_ERROR.value());
    }
}
```

## 5. The Controller

### 5.1. HomeController

```java
@RestController
public class HomeController {

    private static final Logger logger = LoggerFactory.getLogger(GlobalExceptionHandler.class);

    @GetMapping("/")
    public String retResponse(@RequestParam(required = false) String name) {
        logger.info("Call retResponse");

        if (name == null || name.isBlank()) {
            throw new IllegalArgumentException("Please provide your name");
        }
        return String.format("Hey %s, you got a response", name);
    }
}
```

## 6. Run The Application

> Finally, we'll run the Spring Boot application using Maven.
>

```bash
mvn clean spring-boot:run
```

## 7. Check If The API Is Working

```bash
curl http://localhost:8080?name=abhi -w "\n"
```

> If you receive a `200` status code with the response body `Hey abhi, you got a response`, your API is functioning correctly.
>

> To test without required param:
>

```java
curl http://localhost:8080 -w "\n"
```

> You will get an Error Message now, as you didn’t provide the required param, which is desired.
>

```json
{"error":"Bad Request","message":"Please provide your name","errorCode":400}
```



**Here is the [Github Repo](https://github.com/Abhishake63/spring-boot-errorhandling) for this Article!**