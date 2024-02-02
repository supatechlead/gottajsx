--- 
draft : false
date : 2024-02-02T14:37:18+01:00
title : "SSH Config File"
description : "This article covers the basics of the SSH client configuration file"
tags : ["ssh"]
showTableOfContents : true
type: "post"
---

## SSH Config File Location

OpenSSH client-side configuration file is named `config`, and it is stored in the `.ssh` directory under the user’s home directory.

The `~/.ssh` directory is automatically created when the user runs the `ssh` command for the first time. If the directory doesn’t exist on your system, create it using the command below:
```bash
mkdir -p ~/.ssh && chmod 700 ~/.ssh
```

By default, the SSH configuration file may not exist, so you may need to create it using the touch command :
```bash
touch ~/.ssh/config
```

This file must be readable and writable only by the user and not accessible by others:
```bash
chmod 600 ~/.ssh/config
```

## SSH Config File Structure and Patterns

The SSH Config File takes the following structure:
```bash
Host hostname1
    SSH_OPTION value
    SSH_OPTION value

Host hostname2
    SSH_OPTION value

Host *
    SSH_OPTION value
```

The contents of the SSH client config file is organized into stanzas (sections). Each stanza starts with the Host directive and contains specific SSH options used when establishing a connection with the remote SSH server.

Indentation is not required but is recommended since it makes the file easier to read.

The Host directive can contain one pattern or a whitespace-separated list of patterns. Each pattern can contain zero or more non-whitespace character or one of the following pattern specifiers: `*`, `?`, `!`

The SSH config file is also read by other programs such as `scp`, `sftp`, and `rsync`.

