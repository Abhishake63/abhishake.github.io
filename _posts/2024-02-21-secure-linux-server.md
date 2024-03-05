---
layout: post
title: 5 Steps to Secure your Linux Server
subtitle: A Guide to Secure your Linux Server so that you can protect it from Hackers
gh-repo: Abhishake63/abhishake-guides
gh-badge: [star, fork, follow]
tags: [linux, dev-ops]
author: Abhishake Sen Gupta
---

Welcome to our comprehensive guide on securing your Linux server against potential threats and unauthorized access. In this tutorial, we'll walk you through five essential steps to fortify your server's defenses and protect it from hackers.

## 1. Adding a User with Sudo Privileges

> The first step in enhancing server security is to create a new user account with sudo privileges. By adding a non-root user, you reduce the risk associated with using the root account for daily tasks.
>

```bash
adduser admin
echo "admin ALL=(ALL)  ALL" | tee -a /etc/sudoers
```

> Try to ssh into the server from local as user admin with password
>

```bash
ssh admin@hostname
```

## 2. Generating SSH Keys for Secure Authentication

> Generate an SSH key pair and then copy the public key to a remote server
>

```bash
ssh-keygen -t ed25519 -C "your@email.com" -f ~/.ssh/{hostname}
ssh-copy-id -i ~/.ssh/{hostname}.pub admin@hostname
```

> Now, try to login with this command
>

```bash
ssh -i ~/.ssh/{hostname} admin@hostname
```

## 3. Implementing Login Lockdown Measures

> Generally, root login is enabled by default. But we have to change it now for security. Open the sshd-config file by the below command & **change the line ‘#PermitRootLogin yes’ to ‘PermitRootLogin no’**
>

> Generally, the default port SSH listens to is 22. But we have to change it now for security. Open the sshd-config file by the below command & change the line ‘#Port 22’ to ‘Port 1011’
>

> Open the sshd-config file by the below command & change the line ‘#PasswordAuthentication yes’ to ‘PasswordAuthentication no’
>

```bash
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak

nano /etc/ssh/sshd_config

PermitRootLogin no
Port 1011
PasswordAuthentication no

systemctl restart sshd
```

## 4. Configuring a Firewall

> Remember that having a firewall in place is generally considered a good practice for enhancing the security of your system by controlling and monitoring network traffic. However, the specific firewall solution you choose may depend on your preferences, expertise, and the needs of your system.
>

```bash
apt install ufw

ufw allow 1011
```

## 5. Enabling Automatic Updates

> Keeping your server's software up-to-date is essential for patching security vulnerabilities and maintaining system integrity. We'll demonstrate how to configure automatic updates to ensure your server receives timely security patches and bug fixes.
>

```bash
sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

> ***Now, try to login with these command, Your server is secure now***
>

```bash
ssh -i ~/.ssh/{hostname} -p 1011 admin@hostname
```

By following these five steps, you'll significantly enhance the security posture of your Linux server, reducing the risk of unauthorized access and potential security breaches. With a proactive approach to server security, you can safeguard your valuable data and maintain the confidentiality, integrity, and availability of your server resources.

**Here is the [Github Repo](https://github.com/Abhishake63/abhishake-guides) for this Article!**