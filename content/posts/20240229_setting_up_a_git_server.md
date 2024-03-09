--- 
draft : false
date : 2024-02-29T14:37:18+01:00
title : "Setting up a Git Server for Several Users"
tags : ["git"]
showTableOfContents : true
type: "post"
---

Let's go through the steps of setting up an SSH access on the server side. In this example, you will be using the authorized_keys method to authenticate your users. We will also assume that you are using a standard Linux distribution such as Ubuntu.

firstly, we create a `git` user and a `.ssh` directory for this user:
```sh
sudo adduser git
su git
cd
mkdir .ssh && chmod 700 .ssh
touch .ssh/authorized_keys && chmod 600 .ssh/authorized_keys
```
Next, you need to add a developer's public key to the Git user's `authorized_keys` file. Assume you have received a few keys via email and saved them in temporary files. As a reminder, a public key looks like this:
```
cat /tmp/id_rsa.john.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCB007n/ww+ouN4gSLKssMxXnBOvf9LGt4L
ojG6rs6hPB09j9R/T17/x4lhJA0F3FR1rP6kYBRsWj2aThGw6HXLm9/5zytK6Ztg3RPKK+4k
Yjh6541NYsnEAZuXz0jTTyAUfrtU3Z5E003C4oxOj6H0rfIF1kKI9MAQLMdpGW1GYEIgS9Ez
Sdfd8AcCIicTDWbqLAcU4UpkaX8KyGlLwsNuuGztobF8m72ALC/nLF6JLtPofwFBlgc+myiv
O7TCUSBdLQlgMVOFq1I2uPWQOkOWQAHukEOmfjy2jctxSDBQ220ymjaNsHT4kgtZg2AYYgPq
dAv8JggJICUvax2T9va5 gsg-keypair
```
Simply add them to the git user's authorized_keys file in its .ssh directory:
```
cat /tmp/id_rsa.john.pub >> ~/.ssh/authorized_keys
cat /tmp/id_rsa.josie.pub >> ~/.ssh/authorized_keys
cat /tmp/id_rsa.jessica.pub >> ~/.ssh/authorized_keys
```
Now, you can create an empty bare repository by running the `git init` command with the `--bare option`, which initializes a repository without a working directory:

```sh
cd /opt/git
mkdir project.git
cd project.git
git init --bare
```
Then, `John`, `Josie`, or `Jessica` can push the initial version of their project to this repository by adding it as a remote and pushing a branch to it. Note that someone needs to log in to the server shell and create a bare repository for each project addition. Assuming the server's name is gitserver, if you host it internally and have set up DNS to point gitserver to this server, you can use the following commands as is (assuming myproject is an existing project with files):
```sh
# On John computer
cd monproject
git init
git add .
git commit -m 'première validation'
git remote add origin git@gitserveur:/opt/git/projet.git
git push origin master
```
Now, other users can clone the repository and push their changes just as easily:
```sh
git clone git@gitserveur:/opt/git/projet.git
cd projet
vim LISEZMOI
git commit -am 'correction du fichier LISEZMOI'
git push origin master
```
This way, you can quickly set up a read/write Git server for a handful of developers.

It's also worth noting that currently, all these users can connect to the server and get a shell as the 'git' user. If you want to restrict these rights, you'll need to change the shell to something else in the `passwd` file.

You can simply restrict the 'git' user to Git-related actions with a limited shell called `git-shell`, which comes with Git. If you configure this shell as the login shell for the 'git' user, the 'git' user cannot have a normal shell on this server. To use this feature, specify `git-shell` instead of `bash` or `csh` as the user's shell. To do this, you first need to add `git-shell` to `/etc/shells` if it's not already there:
```sh
cat /etc/shells   # To see if `git-shell` is already declared.  Otherwise...
which git-shell   # Ensure that git shell is already installled on the system
sudo vim /etc/shells  # add the complete path to git shell
```
Now, you can edit the user's shell using `chsh <user> -s <shell>`:
```sh
sudo chsh git -s $(which git-shell)
```
Now, the 'git' user can no longer use SSH login for anything other than pushing and pulling from Git repositories; they cannot open a shell anymore. If attempted, you will see a login rejection:
```sh
ssh git@gitserver

fatal: Interactive git shell is not enabled.
hint: ~/git-shell-commands should exist and have read and execute access.
Connection to gitserver closed.
```
Here, users can still use SSH port forwarding to access any host that the git server can access. If you want to prevent this, you can edit the `authorized_keys` file and prefix the following options to any keys you want to restrict:
```sh
no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty
```

The result should look like this:
```sh
cat ~/.ssh/authorized_keys

no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty ssh-rsa
AAAAB3NzaC1yc2EAAAADAQABAAABAQCB007n/ww+ouN4gSLKssMxXnBOvf9LGt4LojG6rs6h
PB09j9R/T17/x4lhJA0F3FR1rP6kYBRsWj2aThGw6HXLm9/5zytK6Ztg3RPKK+4kYjh6541N
YsnEAZuXz0jTTyAUfrtU3Z5E003C4oxOj6H0rfIF1kKI9MAQLMdpGW1GYEIgS9EzSdfd8AcC
IicTDWbqLAcU4UpkaX8KyGlLwsNuuGztobF8m72ALC/nLF6JLtPofwFBlgc+myivO7TCUSBd
LQlgMVOFq1I2uPWQOkOWQAHukEOmfjy2jctxSDBQ220ymjaNsHT4kgtZg2AYYgPqdAv8JggJ
ICUvax2T9va5 gsg-keypair

no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty ssh-rsa
AAAAB3NzaC1yc2EAAAADAQABAAABAQDEwENNMomTboYI+LJieaAY16qiXiH3wuvENhBG...
```
Now, Git network commands will continue to function correctly, but users won't be able to obtain a shell. As the output indicates, you can also configure a directory within the home directory of the `'git'` user that will slightly customize the `git-shell`. For example, you can restrict the Git commands the server accepts, or you can customize the message users will see if they try to SSH in as mentioned above. Run `git help shell` for more information on customizing the shell.