---
layout: post
title: 5 Steps to Secure your Linux Server
subtitle: A Guide to Secure your Linux Server so that you can protect it from Hackers
gh-repo: Abhishake63/Abhishake63
gh-badge: [star, fork, follow]
tags: [linux, dev-ops]
author: Abhishake Sen Gupta
---

## Add An User

> if the only user in the server is the root user then add a user and add sudo privileges to the user

```bash
adduser admin
echo "admin ALL=(ALL)  ALL" | tee -a /etc/sudoers
```

> Try to ssh into the server from local as user admin with password

```bash
ssh admin@hostname
```

## Generating PEM Key

> Generate an SSH key pair and then copy the public key to a remote server

```bash
ssh-keygen -t ed25519 -b 4096 -f ~/.ssh/{hostname}
ssh-copy-id -i ~/.ssh/{hostname}.pub admin@hostname
```

> Copy the id_rsa file to a secure location. Please rename the file so that you can remember like **hostname.pem.** Now, try to login with this command

```bash
ssh -i /path/to/hostname.pem admin@hostname
```

## **Lockdown Logins**

> We will disable Root Login & Password Authentication & will change the Default Port in this section.

> Generally, root login is enabled by default. But we have to change it now for security. Open the sshd-config file by the below command & **change the line ‘#PermitRootLogin yes’ to ‘PermitRootLogin no’**

> Generally, the default port SSH listens to is 22. But we have to change it now for security. Open the sshd-config file by the below command & **change the line ‘#Port 22’ to ‘Port 1011’**

> Open the sshd-config file by the below command & **change the line ‘#PasswordAuthentication yes’ to ‘PasswordAuthentication no’**

```bash
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak

nano /etc/ssh/sshd_config

PermitRootLogin no
Port 1011
PasswordAuthentication no

systemctl restart sshd
```

## Adding a Firewall

> Remember that having a firewall in place is generally considered a good practice for enhancing the security of your system by controlling and monitoring network traffic. However, the specific firewall solution you choose may depend on your preferences, expertise, and the needs of your system.

```bash
apt install ufw

ufw allow 1011
```

## Enable Automatic Updates

```bash
sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

> ***Now, try to login with these command, Your server is secure now***

```bash
ssh -i /path/to/hostname.pem -p 1011 admin@hostname
```