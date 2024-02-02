--- 
draft : false
date : 2024-01-31T14:37:18+01:00
title : "Generate SSH Keys for Github"
description : "Short Reminder for Github SSH Keys Generation"
tags : ["Github", "ssh"]
showTableOfContents : true
type: "post"
---

You can access and write data in repositories on GitHub.com using SSH (Secure Shell Protocol). When you connect via SSH, you authenticate using a private key file on your local machine.

## Generating a New SSH Key

You can generate a new SSH key on your local machine. After you generate the key, you can add the public key to your account on GitHub.com to enable authentication for Git operations over SSH.

1. Open Terminal.

2. Paste the text below, replacing the email used in the example with your GitHub email address.
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
Note: If you are using a legacy system that doesn't support the Ed25519 algorithm, use:
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
This creates a new SSH key, using the provided email as a label.

```
Generating public/private ALGORITHM key pair.
```

When you're prompted to "Enter a file in which to save the key", you can press Enter to accept the default file location. Please note that if you created SSH keys previously, ssh-keygen may ask you to rewrite another key, in which case we recommend creating a custom-named SSH key. To do so, type the default file location and replace id_ALGORITHM with your custom key name.
```
Enter a file in which to save the key (/home/YOU/.ssh/id_ALGORITHM):[Press enter]
```

At the prompt, type a secure passphrase. 
```
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
```

## Adding your SSH Key to the ssh-agent

Before adding a new SSH key to the ssh-agent to manage your keys, you should have checked for existing SSH keys and generated a new SSH key.

1. Start the ssh-agent in the background.
```
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```

Depending on your environment, you may need to use a different command. For example, you may need to use root access by running `sudo -s -H` before starting the ssh-agent, or you may need to use `exec ssh-agent bash` or exec `ssh-agent zsh`` to run the ssh-agent.

2. Add your SSH private key to the ssh-agent.

If you created your key with a different name, or if you are adding an existing key that has a different name, replace id_ed25519 in the command with the name of your private key file.
```bash
ssh-add ~/.ssh/id_ed25519
```

3. Add the SSH public key to your account on GitHub
