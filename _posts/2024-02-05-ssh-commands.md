---
layout: post
title: 6 must-know SSH Commands
subtitle: A Guide to must-know SSH Commands
gh-repo: Abhishake63/Abhishake63
gh-badge: [star, fork, follow]
tags: [linux, dev-ops]
author: Abhishake Sen Gupta
---

Welcome to our guide covering six essential SSH commands that every Linux user should know. SSH (Secure Shell) is a powerful tool for securely accessing remote servers and transferring files between them. Whether you're a beginner or an experienced user, mastering these commands will enhance your efficiency and productivity when working with SSH.

## 1. **Generating SSH Keys**

The first command demonstrates how to generate SSH keys using the **`ssh-keygen`** tool. We provide instructions for generating keys with both the ed25519 and RSA algorithms, along with optional comments to help you identify your keys.

> Nowadays, most platforms recommend you to generate keys with the ed25519 algorithm.
>

```bash
ssh-keygen -t ed25519 -C "your@email.com" -f ~/.ssh/{keyname}
```

> If you prefer to go with RSA for compatibility reasons, use the following:
>

```bash
ssh-keygen -t rsa -b 4096 -C "your@email.com" -f ~/.ssh/{keyname}
```

> The *`-C`* file simply puts a comment on your public key, so you can easily make out which public key belongs to which email address
>

## 2. SSH with Keys

Once you have generated your SSH keys, you can use them to authenticate with remote servers securely. This command illustrates how to specify a specific private key (**`-i`**) when connecting to a remote host using SSH.

```bash
ssh -i mykeyfile user@host
```

## **3. Managing Authorized Keys**

In this section, we explain how to manage authorized keys on remote servers. You'll learn how to append your public key to the **`authorized_keys`** file on the server using both manual and automated methods (**`ssh-copy-id`**).

```bash
cat ~/.ssh/key.pub | ssh user@host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

ssh-copy-id -i ~/.ssh/{hostname}.pub user@host
```

## 4. **Secure Copy (SCP)**

SCP is a command-line utility for securely transferring files between local and remote hosts. We demonstrate how to upload and download files to and from a remote server using SCP, including recursive file transfers.

> To upload a file to a remote server (second command is for recursively uploading):
>

```bash
scp myfile.txt user@dest:/path

scp -rp sourcedirectory user@dest:/path
```

> To download a file to a remote server (second command is for recursively downloading):
>

```bash
scp user@dest:/path/myfile.txt localpath

scp -rp user@dest:/remotedir localpath
```

> `-r` means recursive, `-p` preserves modification times, access times, and modes from the original file.
>

> **Hint**: When doing things recursively via SCP, you might want to consider `rsync`, which also runs over SSH and has [a couple of advantages over SCP](https://serverfault.com/a/264606).
>

```bash
rsync -av /local/dir/ server:/remote/dir/
```

## 5. **SSH Configurations**

Managing SSH configurations can streamline your SSH connections and enhance your workflow. This command shows how to create and customize a **`config`** file in the **`~/.ssh`** directory to define SSH hosts, set connection options, and manage multiple SSH keys.

> Create a file *`~/.ssh/config`* to manage your SSH hosts. Example:
>

```bash
Host targaryen
    HostName 192.168.1.10
    User daenerys
    Port 7654
    IdentityFile ~/.ssh/targaryen.key

Host tyrell
    HostName 192.168.10.20

Host martell
    HostName 192.168.10.50

Host *ell
    user oberyn

Host * !martell
    LogLevel INFO

Host *
    User root
    Compression yes
```

> When you type `ssh targaryen`, the ssh client reads the file and apply the options from the first match, which is `Host targaryen`. Then it checks the next stanzas one by one for a matching pattern. The next matching one is `Host * !martell` (meaning all hosts except `martell`), and it will apply the connection option from this stanza. The last definition `Host *` also matches, but the ssh client will take only the `Compression` option because the `User` option is already defined in the `Host targaryen` stanza.
>

> The full list of options used when you type `ssh targaryen` is as follows:
>

```bash
HostName 192.168.1.10
User daenerys
Port 7654
IdentityFile ~/.ssh/targaryen.key
LogLevel INFO
Compression yes
```

> When running `ssh tyrell` the matching host patterns are: `Host tyrell`, `Host *ell`, `Host * !martell` and `Host *`. The options used in this case are:
>

```bash
HostName 192.168.10.20
User oberyn
LogLevel INFO
Compression yes
```

> If you run `ssh martell`, the matching host patterns are: `Host martell`, `Host *ell` and `Host *`. The options used in this case are:
>

```bash
HostName 192.168.10.50
User oberyn
Compression yes
```

> For all other connections, the ssh client will use the options specified in the `Host * !martell` and `Host *` sections.
>

## 6. Multiple GitHub Keypairs

Managing multiple SSH keys for different GitHub accounts can be challenging. This command provides a solution by demonstrating how to configure SSH aliases in the **`~/.ssh/config`** file to associate specific SSH keys with different GitHub hosts, enabling seamless access to repositories.

> Trying to clone different private GitHub repositories, which have different SSH keypairs associated with them, doesn’t work out of the box.
>

> Add this to your *`.ssh/config`* (this example assumes you have two GitHub keypairs, one for your work account and one for your personal account)
>

```bash
Host github-work.com
    Hostname github.com
    IdentityFile ~/.ssh/id_work

Host github-personal.com
    Hostname github.com
    IdentityFile ~/.ssh/id_personal
```

> Then instead of cloning from *`github.com`*.
>

```bash
git clone git@github.com:abhishake63/abhishake63.git
```

> Clone from either *`github-work.com`* or *`github-personal.com`*.
>

```bash
git clone git@github-work.com:abhishake63/abhishake63.git
```

By mastering these six SSH commands, you'll be better equipped to navigate and leverage the power of SSH for secure remote access and file transfers. Whether you're a sysadmin, developer, or casual Linux user, these commands will undoubtedly prove invaluable in your day-to-day operations.

**Here is the [Github Repo](https://github.com/Abhishake63/abhishake-guides) for this Article!**