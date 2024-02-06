---
layout: post
title: 6 must-know SSH Commands
subtitle: A Guide to must-know SSH Commands
gh-repo: Abhishake63/Abhishake63
gh-badge: [star, fork, follow]
tags: [linux, dev-ops]
author: Abhishake Sen Gupta
---

## Generating Key with ssh-keygen

> Nowadays, most platforms recommend you to generate keys with the ed25519 algorithm.

```bash
ssh-keygen -t ed25519 -C "your@email.com" -f ~/.ssh/{keyname}
```

> If you prefer to go with RSA for compatibility reasons, use the following:

```bash
ssh-keygen -t rsa -b 4096 -C "your@email.com" -f ~/.ssh/{keyname}
```

> The *`-C`* file simply puts a comment on your public key, so you can easily make out which public key belongs to which email address

## SSH with Keys

To use a specific private key to connect to a server, use:

```bash
ssh -i mykeyfile user@host
```

## **Authorized Keys**

> For servers, you simply need to append your public key to the file *`~/.ssh/authorized_keys`*.

```bash
cat ~/.ssh/key.pub | ssh USER@HOST "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

ssh-copy-id -i ~/.ssh/{hostname}.pub user@host
```

## SCP

> To upload a file to a remote server (second command is for recursively uploading):

```bash
scp myfile.txt user@dest:/path

scp -rp sourcedirectory user@dest:/path
```

> To download a file to a remote server (second command is for recursively downloading):

```bash
scp user@dest:/path/myfile.txt localpath

scp -rp user@dest:/remotedir localpath
```

> `-r` means recursive, `-p` preserves modification times, access times, and modes from the original file.

> **Hint**: When doing things recursively via SCP, you might want to consider `rsync`, which also runs over SSH and has [a couple of advantages over SCP](https://serverfault.com/a/264606).

```bash
rsync -av /local/dir/ server:/remote/dir/
```

## SSH Config

> Create a file *`~/.ssh/config`* to manage your SSH hosts. Example:

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

> The full list of options used when you type `ssh targaryen` is as follows:

```bash
HostName 192.168.1.10
User daenerys
Port 7654
IdentityFile ~/.ssh/targaryen.key
LogLevel INFO
Compression yes
```

> When running `ssh tyrell` the matching host patterns are: `Host tyrell`, `Host *ell`, `Host * !martell` and `Host *`. The options used in this case are:

```bash
HostName 192.168.10.20
User oberyn
LogLevel INFO
Compression yes
```

> If you run `ssh martell`, the matching host patterns are: `Host martell`, `Host *ell` and `Host *`. The options used in this case are:

```bash
HostName 192.168.10.50
User oberyn
Compression yes
```

> For all other connections, the ssh client will use the options specified in the `Host * !martell` and `Host *` sections.

## **Multiple GitHub Keypairs**

> Trying to clone different private GitHub repositories, which have different SSH keypairs associated with them, doesn’t work out of the box.

> Add this to your *`.ssh/config`* (this example assumes you have two GitHub keypairs, one for your work account and one for your personal account)

```bash
Host github-work.com
    Hostname github.com
    IdentityFile ~/.ssh/id_work

Host github-personal.com
    Hostname github.com
    IdentityFile ~/.ssh/id_personal
```

> Then instead of cloning from *`github.com`*.

```bash
~~git clone git@github.com:abhishake63/abhishake63.git~~
```

> Clone from either *`github-work.com`* or *`github-personal.com`*.

```bash
git clone git@github-work.com:marcobehlerjetbrains/buildpipelines.git
```