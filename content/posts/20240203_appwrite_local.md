--- 
draft : false
date : 2024-02-03T14:37:18+01:00
title : "Create a Local Appwrite Instance"
description : "This tutorial explains how to package Appwrite inside a Docker instance"
tags : ["Docker", "Appwrite"]
showTableOfContents : true
type: "post"
---

The Appwrite server is packaged as a set of Docker containers. The Docker approach allows you to install and run Appwrite easily on any operating system (OS) that can run a Docker CLI.

## Prerequisite

To run Appwrite with Docker, you must have a machine with:

* At least 4GB of RAM.
* 64-bit processor
* An OS that supports Docker - Linux, Windows, or macOS.

Appwrite requires Docker Compose Version 2. To install Appwrite, make sure your Docker installation is updated to support Composer V2.

## Install with Docker

The easiest way to start running your Appwrite server is by running our Docker installer tool from your terminal. Before running the installation command, make sure you have Docker CLI installed on your host machine.

Appwrite can be installed with a single Docker command. Essentially, this command has Docker simultaneously download and start running the Appwrite services.

The command creates a new appwrite directory in the current directory, and this new directory becomes the base for your Appwrite instance:
```bash
docker run -it --rm \
    --volume /var/run/docker.sock:/var/run/docker.sock \
    --volume "$(pwd)"/appwrite:/usr/src/code/appwrite:rw \
    --entrypoint="install" \
    appwrite/appwrite:1.4.13
```
For the Docker run references in the command:
* The `-it` flag tells the Docker CLI to open an interactive container instance of the image.
* The `--rm` flag removes the container automatically after you close Docker.
* The `--volume` flag mounts a unique volume on your machine to a path in the container. The two paths are separated by `:`.
* The first `--volume` flag bind mounts your machine daemon’s socket (`/var/run/docker.sock`) with the container’s daemon socket to manage other containers through the Appwrite container.
* The second `--volume` flag mounts the `appwrite` folder created in your current working directory to the running container for data persistence.
* The -`-entrypoint` flag tells Docker to execute the "install" command when the container starts.
* The last line of the command specifies the version of the Appwrite image to pull from Docker Hub. To know the latest or past image releases, check out the Appwrite releases on GitHub.

You will be prompted to configure the following during the setup command:

1. Your Appwrite instance's HTTP and HTTPS ports.

2. Your Appwrite instance's secret key used to encrypt sensitive data.

3. Your Appwrite instance's main hostname. Appwrite will generate a certificate using this hostname.

4. Your Appwrite instance's DNS A record hostname. Typically set to the same value as your Appwrite instance's hostname.

Docker then runs a Docker Compose command to complete the setup. When this is finished, you should see:
```bash
Appwrite installed successfully
```

## Starting Appwrite

Appwrite is now up and running. You can access the dashboard by navigating to http://localhost in a web browser on the server.

However, doing so is often not feasible. Likely, you want to access the dashboard remotely. You can do so by navigating to the Appwrite server’s URL, which may be an IP address, such as 192.0.2.0. But first, you need to ensure the server’s firewall provides external access to the HTTP port (80).

## How to Manage the Appwrite Instance

There are a few more things you can do to set up and further configure your Appwrite instance. Additionally, you may want to control the Appwrite services. For instance, turning them on and off as needed for server maintenance.

These next sections give you some of the most useful tools for configuring and managing your Appwrite instance.

### Controlling Appwrite Services

Appwrite runs using `Docker Compose`, which you can use to control Appwrite’s state. To do so, you need to be in the `Appwrite` directory. From there, you can use the `Docker Compose` commands to control the Appwrite services.

These are some examples of useful `Docker Compose` commands for working with your Appwrite instance.

* To restart the Appwrite services:
```bash
sudo docker compose restart
```

* To stop the services:
```bash
sudo docker compose stop
```

* To stop and uninstall the services:
```bash
sudo docker compose down -v
```
This option leaves the Docker Compose files for Appwrite intact, so you can reinstall and restart Appwrite with the next command.

* To start the services back up:
```bash
sudo docker compose up -d
```

### Configuring Appwrite

Appwrite’s configurations are primarily controlled by environmental variables set in the `.env` file stored in the appwrite directory that was created when you installed Appwrite.

To make a change, take these steps, executing the commands while in the Appwrite directory.

1. Stop the Appwrite services:
```bash
sudo docker compose stop
```

2. Use your preferred text editor to make the necessary changes to the `.env` file.

3. Start the services up again:
```bash
sudo docker compose up -d
```
You can see the full range of environmental variables Appwrite uses, each with a helpful description, in the official documentation.

