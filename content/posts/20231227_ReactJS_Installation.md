---
title: "ReactJS Installation"
date: 2023-12-27T17:41:05+01:00
type: "post"
draft: false 
description: "Guide for ReactJS installation on Ubuntu Linux"
showTableOfContents: true
tags: ["Installation", "ReactJS", "Ubuntu"]
---
Developed by Facebook in 2011, React (also referred to as ReactJS) is a Javascript library used for creating fast and interactive user interfaces. At the time of writing, it’s the most popular Javascript library for developing user interfaces.

In this article, you will learn how to install ReactJS on Ubuntu 18.04 and latest.

## Installing Node.js and npm using NVM

NVM (Node Version Manager) is a bash script that allows you to manage multiple Node.js versions on a per-user basis. With NVM, you can install and uninstall any Node.js version that you want to use or test.

Visit the [nvm GitHub repository](https://github.com/nvm-sh/nvm#installing-and-updating) page and copy either the `curl` or `wget` command to download and install the `nvm` script:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```
Do not use `sudo` , as it will enable nvm for the root user.

The script will clone the project’s repository from GitHub to the `~/.nvm` directory:
```
...
=> Close and reopen your terminal to start using nvm, or run the following to use it now:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```
As the output above says, you should either close and reopen the terminal or run the commands to add the path to `nvm` script to the current shell session. You can do whatever is easier for you.

Once the script is in your `PATH`, verify that `nvm` was properly installed by typing:
```bash
nvm --version
```
To get a list of all Node.js versions that can be installed with `nvm`, run:
```bash
nvm list-remote
```
The command will print a huge list of all available Node.js versions.

To install the latest available version of Node.js, run:
```bash
nvm install node
```
Once the installation is completed, verify it by printing the Node.js version:
```bash
node --version
```
## Create and Launch Your First React Application

Create React App is an officially supported way to create single-page React applications. It offers a modern build setup with no configuration.
```bash
npx create-react-app my-app
cd my-app
npm start
```
> If you've previously installed `create-react-app` globally via `npm install -g create-react-app`, we recommend you uninstall the package using `npm uninstall -g create-react-app` or `yarn global remove create-react-app` to ensure that `npx` always uses the latest version.

npx comes with npm 5.2+ and higher.

Then open http://localhost:3000/ to see your app.

When you’re ready to deploy to production, create a minified bundle with npm run build.

