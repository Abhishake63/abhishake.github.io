---
layout: post
title: Securing Spring Boot APIs With API Key and Secret
subtitle: A Comprehensive Guide to Secure Spring Boot APIs With API Key and Secret
gh-repo: Abhishake63/spring-security-api
gh-badge: [star, fork, follow]
tags: [spring-boot, java, spring-security]
author: Abhishake Sen Gupta
---

Welcome to our guide on securing Spring Boot APIs using API key and secret authentication! In this comprehensive tutorial, we'll walk you through the process of implementing robust security measures to protect your APIs from unauthorized access.

## 1. Overview

This guide provides a foundation for creating a Spring Boot application with Spring Security for securing APIs using API key and secret authentication.

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
  <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

## **4. Configure Spring Security**

> Configure Spring Security to handle API key and secret authentication.
>

### 4.1. ApiKeyAuthFilter

```java
@Component
public class ApiKeyAuthFilter extends OncePerRequestFilter {

    private AuthenticationService authenticationService;

    ApiKeyAuthFilter(AuthenticationService authenticationService){
        this.authenticationService = authenticationService;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
            throws ServletException, IOException {

        if (checkIfApiKeyAndSecretPresentInHeader(request)) {
            Authentication authentication = authenticationService.getAuthentication(request);
            SecurityContextHolder.getContext().setAuthentication(authentication);
        }
        filterChain.doFilter(request, response);
    }

    private Boolean checkIfApiKeyAndSecretPresentInHeader(HttpServletRequest request) {

        String requestApiKey = request.getHeader("X-API-KEY");
        String requestApiSecret = request.getHeader("X-API-SECRET");
        if (requestApiKey == null && requestApiSecret == null) {
            return false;
        }
        return true;
    }
}
```

### 4.2. AuthenticationService

```java
@Service
public class AuthenticationService {

    private String apiKey = "MY-KEY";
    private String apiSecret = "MY-SECRET";

    public Authentication getAuthentication(HttpServletRequest request) {

        String requestApiKey = request.getHeader("X-API-KEY");
        String requestApiSecret = request.getHeader("X-API-SECRET");

        if (!apiKey.equals(requestApiKey) || !apiSecret.equals(requestApiSecret)) {
            throw new BadCredentialsException("Invalid API Key");
        }
        return new ApiKeyAuthentication(apiKey, AuthorityUtils.NO_AUTHORITIES);
    }
}
```

### 4.3. ApiKeyAuthentication

```java
public class ApiKeyAuthentication extends AbstractAuthenticationToken {

    private final String apiKey;

    public ApiKeyAuthentication(String apiKey, Collection<? extends GrantedAuthority> authorities) {
        super(authorities);
        this.apiKey = apiKey;
        setAuthenticated(true);
    }

    @Override
    public Object getCredentials() {
        return null;
    }

    @Override
    public Object getPrincipal() {
        return apiKey;
    }
}
```

### 4.4. SecurityConfig

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    private ApiKeyAuthFilter apiKeyAuthFilter;
    private AuthExceptionHandler authExceptionHandler;

    SecurityConfig(ApiKeyAuthFilter apiKeyAuthFilter, AuthExceptionHandler authExceptionHandler) {
        this.apiKeyAuthFilter = apiKeyAuthFilter;
        this.authExceptionHandler = authExceptionHandler;
    }

    @Bean
    SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {

        http
        .csrf(AbstractHttpConfigurer::disable)
        .exceptionHandling(exceptionHandling -> exceptionHandling
            .authenticationEntryPoint(authExceptionHandler)
        )
        .sessionManagement(sessionManagement -> sessionManagement
            .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
        )
        .authorizeHttpRequests(authorize -> authorize
            .requestMatchers("/not-secured").permitAll()
            .anyRequest().authenticated()
        )
        .addFilterBefore(apiKeyAuthFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
}
```

### 4.5. **Disabling the Default Auto-Configuration**

```java
@SpringBootApplication(exclude = {SecurityAutoConfiguration.class, UserDetailsServiceAutoConfiguration.class})
public class ApiSecurityApplication {

	public static void main(String[] args) {
		SpringApplication.run(ApiSecurityApplication.class, args);
	}
}
```

## 5. The Controller

### 5.1. HomeController

```java
@RestController
public class HomeController {

    @GetMapping("/")
    public String homeEndpoint() {
        return "Your api is secure !";
    }

    // curl -H "X-API-KEY: MY-KEY" -H "X-API-SECRET: MY-SECRET" http://localhost:8080/ -w "\n"

    @GetMapping("/not-secured")
    public String notSecure() {
        return "not secure";
    }

    // curl http://localhost:8080/not-secured -w "\n"
}
```

## 6. Run The Application

> Finally, we'll run the Spring Boot application using Maven.
>

```bash
mvn clean spring-boot:run
```

## 7. Check If The API Is Secure

```bash
curl -H "X-API-KEY: MY-KEY" -H "X-API-SECRET: MY-SECRET" http://localhost:8080/ -w "\n"
```

> If you receive a `200` status code with the response body `Your api is secure !`, your API is functioning correctly.
>

> To test without `API Key & Secret`:
>

```java
curl http://localhost:8080/ -w "\n"
```

> You won’t be able to access the protected resource now.
>

**Here is the [Github Repo](https://github.com/Abhishake63/spring-security-api) for this Article!**