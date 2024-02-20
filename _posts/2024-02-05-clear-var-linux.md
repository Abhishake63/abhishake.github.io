---
layout: post
title: How to Clear /var in Linux
subtitle: A Guide to cleaning /var directory in your Linux machine
gh-repo: Abhishake63/abhishake-guides
gh-badge: [star, fork, follow]
tags: [linux]
author: Abhishake Sen Gupta
---

Welcome to our guide on clearing the **`/var`** directory in Linux, where we'll explore essential commands and techniques to free up disk space and optimize system performance. The **`/var`** directory contains various files that can accumulate over time and consume valuable disk space. By following the steps outlined in this guide, you'll learn how to efficiently manage and clean up the **`/var`** directory, ensuring optimal operation of your Linux system.

## 1. Checking Disk Space

> Before cleaning, check your disk space usage to identify if **`/var`** is consuming a significant amount. You can use the **`df`** command to display disk space information:
>

```bash
df -h
```

## 2. Removing Unnecessary Packages

> This command removes packages that were automatically installed to satisfy dependencies but are no longer needed.
>

```bash
sudo apt-get autoremove
```

## 3. Cleaning APT Cache

> This clears the APT package cache, which contains downloaded package files. It helps free up disk space.
>

```bash
sudo du -sh /var/cache/apt # see the size of cache you have
sudo apt-get clean  # to clean the cache
```

## 4. Managing Systemd Journal Logs

> This manages the systemd journal logs, displaying usage information and vacuuming old logs to free up space.
>

```bash
sudo journalctl --disk-usage # check the storage used in old logs
sudo journalctl --vacuum-time=3d # to clear logs before 3 days - number before d can be changed
```

## 5. Clearing Thumbnail Cache

> This deals with the thumbnail cache, which stores previews of images and videos. Clearing it can help in freeing up some space.
>

```bash
du -sh ~/.cache/thumbnails # check the thumbnails size
rm -rf ~/.cache/thumbnails/* # clean the thumbnails
```

## 6. Cleaning RAM Cache

> This command clears the RAM cache. It's worth noting that clearing the cache can impact system performance temporarily, and it's typically not necessary for regular system maintenance.
>

```bash
top # check ram cache
sudo sync && sudo sh -c 'echo 3 > /proc/sys/vm/drop_caches'
```

By following these steps, you'll be able to effectively manage and clean up the **`/var`** directory in Linux, ensuring optimal disk space usage and system performance. Whether you're a sysadmin, developer, or Linux enthusiast, mastering these techniques will help keep your system running smoothly.

**Here is the [Github Repo](https://github.com/Abhishake63/abhishake-guides) for this Article!**