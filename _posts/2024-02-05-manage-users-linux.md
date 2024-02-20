---
layout: post
title: How to Manage Users in Linux
subtitle: A Guide to managing users in your Linux Server
gh-repo: Abhishake63/abhishake-guides
gh-badge: [star, fork, follow]
tags: [linux]
author: Abhishake Sen Gupta
---

Welcome to our comprehensive guide on managing users in Linux, where we'll explore essential commands and files related to user management in Unix-like operating systems. Whether you're a system administrator, developer, or Linux enthusiast, understanding how to effectively manage users is fundamental to maintaining system security and organization.

## 1. Exploring User Files

We'll begin by delving into two crucial files: **`/etc/passwd`** and **`/etc/shadow`**. These files store essential information about user accounts, including usernames, encrypted passwords, user IDs (UIDs), group IDs (GIDs), and more. Understanding the structure and content of these files is key to managing users effectively.

### 1.1. Etsy Password File

```bash
cat /etc/passwd
```

> The **`/etc/passwd`** file in Linux and Unix-like operating systems is a text file that contains essential information about user accounts. Each line in the file represents a user account and is structured with colon-separated fields. The typical format of the **`/etc/passwd`** file is as follows:
>

```bash
username:password:UID:GID:GECOS:home_directory:login_shell
```

> To know the number of users in a system
>

```bash
cat /etc/passwd | wc -l
```

> To find a user from that file
>

```bash
cat /etc/passwd | grep username
```

> You will see a line like this
>

```bash
abhishake:x:1000:1000:abhishake,,,:/home/abhishake:/bin/bash
```

### 1.2. Etsy Shadow File

```bash
sudo cat /etc/shadow | grep username
```

> The **`/etc/shadow`** file in Linux and Unix-like operating systems stores the encrypted password and other security-related information for user accounts. It is a critical file for protecting user passwords, and its permissions are typically set to be readable only by the root user.
>

```bash
username:password:last_change:min_age:max_age:warning:inactive:expire:disable:reserved
```

> Here is an example line from the **`/etc/shadow`** file:
>

```bash
abhishake:$y$j9T$LOBsz9MWvNpM7HIA8FXl01$un2uvIgy4d:19158:0:99999:7:::
```

## 2. Adding & Deleting Users

> To create the user's home directory, use the **`-m`** option with **`useradd`**:
>

```bash
sudo useradd -m username
```

> This command creates a user without interactive prompts. To set a password for the user, you can use the **`passwd`** command:
>

```bash
sudo passwd username
```

> Check if it created a home directory
>

```bash
ls -l /home/
```

> To add a system user
>

```bash
sudo useradd -r sysuser
```

> To delete a user with the home directory
>

```bash
sudo userdel -r username
```

Whether you're responsible for managing a single-user system or an enterprise-level server, this guide will equip you with the knowledge and skills needed to effectively manage users in Linux. By following these best practices, you can ensure the security, stability, and efficiency of your Linux environment.

**Here is the [Github Repo](https://github.com/Abhishake63/abhishake-guides) for this Article!**