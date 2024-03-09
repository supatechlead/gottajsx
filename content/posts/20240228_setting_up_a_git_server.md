--- 
draft : false
date : 2024-02-28T14:37:18+01:00
title : "Setting up a Git Server on Linux"
tags : ["git"]
showTableOfContents : true
type: "post"
---

Git is a popular version control system that keeps track of the modifications made to a particular set of files. There are a lot of platforms on the Internet that we can use to host Git repositories, such as GitHub, GitLab, Bitbucket, and more. Alternatively, we can set up a Git server on a Linux system to do the job.

A Git server is basically a way to host Git repos privately. In this tutorial, we’ll discuss the step-by-step process that we can follow to set up a Git server on Linux.

Installing Git
Firstly, we’ll install Git on the system using the `apt` and `dnf` commands as the main package managers used on many major distributions. Moreover, we have to be a `root` user to perform the installations, so we’ll use the sudo command.

On Debian-based distros such as Ubuntu, we use the apt command:
```
sudo apt install git
```
In the case of Fedora-based distros such as RHEL, we go with `dnf`:
```
sudo dnf install git
```
Now, let’s configure the system to make it act like a Git server.

## Creating a New User

The first step is to create a new user that will manage the Git repos on the server:
```
sudo adduser git
```

The `adduser` command adds a new user to the system. The string that we see after it, i.e., `git`, is the name of the user.

Upon entering the command, the user account creation process commences:
```
Adding user ‘git’ ...
Adding new group ‘git’ (1602) ...
Adding new user ‘git’ (1002) with group ‘git' ...
Creating home directory ‘/home/git' ...
Copying files from '/etc/skel' ...
New password:
Retype new password:
passwd: password updated successfully
Is the information correct? [Y/n] y
```
As we can see above, we have to create a password for logging in as the new user and accessing the server. However, we’ll use SSH in this case to do the operation. So we’ll bypass the password prompt by hitting `Enter` on the terminal.

Then, we’ll type `y (yes)` on the next prompt to confirm the provided information and finish the account creation process.

Before moving on, we’ll take note of the group name, i.e., `git`, and the home directory, i.e., `/home/git`, for future use.

## Creating a Repo on the Git Server

In this step, we’ll create a directory that will store all the Git repositories on the server. So, let’s use the `mkdir` command to create one named storage inside the new user’s home directory:

```
sudo mkdir /home/git/storage
```
Secondly, we’ll use the chown command to change the owner of the directory to the user:
```
sudo chown git:git /home/git/storage
```
We’ve specified the user using the `git:git` string, in which the name (the value on the left) and group name (the value on the right) are separated by a colon (`:`).

Finally, we’ll switch to the user using the `su` command:
```
su -l git
```
The `-l` option, short for `–login`, enables us to log in to the server as the user, specified using its username.

So, we’re pretty much done transforming the system into a Git server. Now, let’s start preparing the server for development.

## Initializing a Git Repo on the Git Server

We’ll first initialize an empty Git repository in the server’s storage directory using the `git init` command:
```
cd storage
git init --bare
```

The `–bare` option creates a bare Git repo instead of a normal one. We use a bare repo in cases where we want to push and pull code from a repo but don’t want to perform development in there. Since we want the repo on the server to be exactly like this, we’ve initialized it as a bare one.

## Configuring SSH Access

The next step is to configure SSH access for the new user, which will enable us to connect to the server as the user via SSH.

So let’s generate an SSH key pair for the user using the `ssh-keygen` command:
```
ssh-keygen rsa

Generating public/private rsa key pair.
Enter file in which to save the key (/home/git/.ssh/id_rsa):
Created directory '/home/git/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/git/.ssh/id_rsa
Your public key has been saved in /home/git/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:3KinvrEPZHAFA42+VUIWOhuKPhINnSMQ7u6n5C15eXE
```
The command generates the key pair using the RSA algorithm, specified at the end.

We’ll be prompted to enter a file for saving the private key, for which we’ll hit Enter to go with the default option. Moreover, we’ll not use a passphrase in this case, so we’ll again hit Enter to skip the passphrase prompt and finish the process of SSH key pair generation.

Evidently, the public key is saved in the `id_rsa.pub` file located in the `.ssh` directory. So, we’ll now navigate to the directory, open the file using a text editor, and copy the key inside it.

Then, we’ll use the touch command to create a file named `authorized_keys` inside `.ssh`:
```
touch authorized_keys
```

After creating the file, we’ll open it using a text editor and paste the previously copied key into it.

Now, we can use SSH to connect to the server as the new user. However, we don’t yet have the permission to do it. So let’s change that in the next step.

## Getting Permission to Connect via SSH

Firstly, we’ll navigate to the `/etc/ssh` directory and open the `sshd_config` file using a text editor. Then, we’ll add a line at the end of the file:
```
AllowUsers git
```
The `AllowUsers` keyword gives us permission to connect to the server via SSH as the new user.

Now, we apply the change by restarting the SSH service:
```
sudo systemctl ssh restart
```
The `systemctl` and `restart` commands restart the SSH service, written as `ssh`. As a result, we’re now finally eligible to use SSH for connecting to the server as the user.

## Cloning the Repository From the Git Server to the Client

The last step is to clone the repository on the server, i.e., storage, to a particular location on the local system. So, let’s navigate to the `usr` directory on the client machine and use the `git clone` command to clone the repo to the directory:
```
cd usr
git clone git@server:/home/git/storage
```

We’ve connected to the server using the `git@server` string, where `git` is the username of the new user and `server` is the hostname of the Git server. In the case of the hostname, we can set its value to anything. Moreover, we’ve specified the storage repo using its full path, which is separated from `git@server` by a colon (`:`).

Now, we have the storage repo on both the local system and the server. Since the local repo is cloned from the server repo, Git has automatically added the server repo to the local repo as a remote, i.e., a location where the local repo is hosted. This indicates that both of them are now connected to each other, which means that we can finally start using the server for development.