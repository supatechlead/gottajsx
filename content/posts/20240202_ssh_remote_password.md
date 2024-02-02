--- 
draft : false
date : 2024-02-01T14:37:18+01:00
title : "SSH Remote Password"
description : "How to SSH to a Remote server Without Typing your Password"
tags : ["ssh"]
showTableOfContents : true
type: "post"
---

Follow the steps below:

## First Step

Run this Bash script on your laptop:
```bash
#!/usr/bin/env bash

# The hostname of your remote server.
host=myserver.com

# Create this folder if it does not exist: ~/.ssh
mkdir ~/.ssh

# Set the correct permissions (required)
chmod 700 ~/.ssh

# Generate an RSA key pair for identification with the remote server
ssh-keygen -t rsa

# Copy your public key to the remote server
cat ~/.ssh/id_rsa.pub | ssh $host 'cat >> ~/.ssh/authorized_keys'

# ssh is very strict about correct permissions
ssh $host 'chmod g-w,o-w ~; chmod 700 ~/.ssh; chmod 600 ~/.ssh/authorized_keys'
```
At this point, you can connect to your server without typing a password:
```bash
ssh username@myserver.com
```

## Alias Server

If you would prefer to type `jupiter` (7 characters) rather than `username@myserver.com` (21 characters), create a config file `~/.ssh/config` on your laptop, as shown below. You'll be able to use the `jupiter` alias with: `rsync`, `scp`, and `ssh`.
```bash
# ~/.ssh/config

Host jupiter
    User carl
    HostName myserver.com
```

At this point, you can connect to your server like this:
```bash
ssh jupiter
```

## Alias Commands

If you want a single command to do two steps:
* First, connect to a login server jupiter
* Next, connect to a work server saturn

Then configure your `~/.ssh/config` file as follows:
```bash
# ~/.ssh/config

Host jupiter
    User carl
    Hostname myserver.com

Host saturn
    User carl
    ProxyCommand ssh -qX jupiter nc %h %p
```
At this point, you can connect to saturn via `jupiter` like this:
```bash
ssh saturn
```
