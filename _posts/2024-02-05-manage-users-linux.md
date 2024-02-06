---
layout: post
title: How to Manage Users in Linux
subtitle: A Guide to managing users in your Linux Server
gh-repo: Abhishake63/Abhishake63
gh-badge: [star, fork, follow]
tags: [linux]
author: Abhishake Sen Gupta
---

## Etsy Password File

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

## Etsy Shadow File

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

## Adding & Deleting Users

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

To delete a user with the home directory

```bash
sudo userdel -r username
```