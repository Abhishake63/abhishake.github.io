---
layout: post
title: How to Send Emails with Spring Boot
subtitle: A Comprehensive Guide to Spring Email
gh-repo: Abhishake63/spring-boot-email
gh-badge: [star, fork, follow]
tags: [spring-boot, java]
author: Abhishake Sen Gupta
---

Welcome to the Spring Boot "Email Sender" project! This is a simple project that demonstrates the way of sending emails from a Spring Boot Application.

## 1. Overview

This project serves as a foundational framework for developing an email sending application using modern technologies and best practices. It provides a comprehensive setup and essential dependencies to swiftly launch your email sender project, allowing you to focus on crafting robust email functionality without the hassle of intricate setup procedures.

With this project as your starting point, you'll delve into the intricacies of email communication while leveraging the power and flexibility of the chosen technologies. Whether you're a seasoned developer or just embarking on your coding journey, this project will empower you to build a reliable and efficient email sending application with ease.

## 2. Prerequisites

Before you begin, ensure you have met the following requirements:

- Java Development Kit (JDK) installed (version 17 or higher)
- Maven build tool installed

## 3. Maven Dependencies

> First, we need to add the dependencies to our `pom.xml.`
>

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
  <groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
  <optional>true</optional>
</dependency>
```

## 4. Mail Server Properties

```java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    @Value("${spring.mail.host}")
    private String MAIL_HOST;

    @Value("${spring.mail.port}")
    private int MAIL_PORT;

    @Value("${spring.mail.username}")
    private String MAIL_USERNAME;

    @Value("${spring.mail.password}")
    private String MAIL_PASSWORD;

    @Bean
    JavaMailSender getJavaMailSender() {
        Properties mailProperties = new Properties();
        mailProperties.put("mail.transport.protocol", "smtp");
        mailProperties.put("mail.smtp.auth", true);
        mailProperties.put("mail.smtp.ssl.enable", true);
        mailProperties.put("mail.smtp.ssl.required", true);
        mailProperties.put("mail.smtp.debug", true);
        mailProperties.put("mail.smtp.socketFactory.class", "javax.net.ssl.SSLSocketFactory");
        mailProperties.put("mail.smtp.socketFactory.fallback", false);

        JavaMailSenderImpl mailSender = new JavaMailSenderImpl();
        mailSender.setJavaMailProperties(mailProperties);
        mailSender.setHost(MAIL_HOST);
        mailSender.setPort(MAIL_PORT);
        mailSender.setUsername(MAIL_USERNAME);
        mailSender.setPassword(MAIL_PASSWORD);

        return mailSender;
    }
}
```

> To send emails using Gmail, we set our `application.properties`:
>

```
server.port=8081

# mail setup

spring.mail.host=smtp.gmail.com
spring.mail.port=587

spring.mail.username=<USERNAME>
spring.mail.password=<PASSWORD>
```

> This is your `application.properties` file. Replace `USERNAME` with your gmail username, and `PASSWORD` with the correct mail password
>

## 5. Sending Emails

> As dependency management and configuration are in place, we can use the `JavaMailSender` to send an email.
>

### 5.1. Model

```java
@Getter
@Setter
@AllArgsConstructor
public class EmailDetails {

    private String to;
    private String body;
    private String subject;
}
```

### 5.2. Implementing MailServiceImpl

```java
@Service
public class MailServiceImpl implements MailService {

    private JavaMailSender javaMailSender;

    public MailServiceImpl(JavaMailSender javaMailSender) {
        this.javaMailSender = javaMailSender;
    }

    @Value("${spring.mail.username}")
    private String sender;

    @Override
    public Boolean sendMail(EmailDetails emailDetails) {
        try {
            SimpleMailMessage mailMessage = new SimpleMailMessage();
            mailMessage.setFrom(sender);
            mailMessage.setTo(emailDetails.getTo());
            mailMessage.setText(emailDetails.getBody());
            mailMessage.setSubject(emailDetails.getSubject());
            javaMailSender.send(mailMessage);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    @Override
    public Boolean sendMailWithAttachment(String to, String subject, String text, InputStreamSource attachementFile, String attachementFileName) {
        try {
            MimeMessage message = javaMailSender.createMimeMessage();
            MimeMessageHelper helper = new MimeMessageHelper(message, true);
            helper.setFrom(sender);
            helper.setTo(to);
            helper.setSubject(subject);
            helper.setText(text);
            helper.addAttachment(attachementFileName, attachementFile);
            javaMailSender.send(message);
            return true;
        } catch (MessagingException ex) {
            ex.printStackTrace();
            return false;
        }
    }
}
```

### 5.3. The Controller

```java
@RestController
@RequestMapping("/api")
public class EmailController {

    private MailService mailService;

    public EmailController(MailService mailService) {
        this.mailService = mailService;
    }

    @PostMapping("/sendMail")
    public Boolean sendMail(@RequestBody EmailDetails emailDetails) {
        return mailService.sendMail(emailDetails);
    }

    // curl -X POST -H 'Content-Type: application/json' -d '{"to": "<RECIPIENT_MAIL>", "subject": "test subject", "body": "test body"}' http://localhost:8081/api/sendMail -w "\n"
}
```

## 6. Run The Application & Test

### 6.1. Run

```bash
mvn clean spring-boot:run
```

### 6.2. Check if the microservice can send Mails

```bash
curl -X POST -H 'Content-Type: application/json' -d '{"to": "<RECIPIENT_MAIL>", "subject": "test subject", "body": "test body"}' http://localhost:8081/api/sendMail -w "\n"
```

> Replace the `RECIPIENT_MAIL` in the request body with a valid email.  If you get a `200` status code with a response body `true`, your mail microservice is succesfully working. Also check your recipient mail if you got the email or not.
>

**Here is the [Github Repo](https://github.com/Abhishake63/spring-boot-email) for this Article!**