---
layout: post
title: How to Clear /var in Linux
subtitle: A Guide to cleaning /var directory in your Linux machine
gh-repo: Abhishake63/Abhishake63
gh-badge: [star, fork, follow]
tags: [linux]
author: Abhishake Sen Gupta
---

## 1. Check Disk Space

> Before cleaning, check your disk space usage to identify if **`/var`** is consuming a significant amount. You can use the **`df`** command to display disk space information:
>

```bash
df -h
```

## 2. Get rid of packages that are no longer required

> This command removes packages that were automatically installed to satisfy dependencies but are no longer needed.
>

```bash
sudo apt-get autoremove
```

## 3. Clean up APT cache

> This clears the APT package cache, which contains downloaded package files. It helps free up disk space.
>

```bash
sudo du -sh /var/cache/apt # see the size of cache you have
sudo apt-get clean  # to clean the cache
```

## 4. Clear systemd journal logs

> This manages the systemd journal logs, displaying usage information and vacuuming old logs to free up space.
>

```bash
sudo journalctl --disk-usage # check the storage used in old logs
sudo journalctl --vacuum-time=3d # to clear logs before 3 days - number before d can be changed
```

## 5. Clean the thumbnail cache

> This deals with the thumbnail cache, which stores previews of images and videos. Clearing it can help in freeing up some space.
>

```bash
du -sh ~/.cache/thumbnails # check the thumbnails size
rm -rf ~/.cache/thumbnails/* # clean the thumbnails
```

## 6. Clean up RAM cache

> This command clears the RAM cache. It's worth noting that clearing the cache can impact system performance temporarily, and it's typically not necessary for regular system maintenance.
>

```bash
top # check ram cache
sudo sync && sudo sh -c 'echo 3 > /proc/sys/vm/drop_caches'
```