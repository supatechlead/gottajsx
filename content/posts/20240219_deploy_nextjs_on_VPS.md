--- 
draft : false
date : 2024-02-19T14:37:18+01:00
title : "How to Deploy a Next.js app to a Custom Server"
tags : ["plantuml","Hugo"]
showTableOfContents : true
type: "post"
---

## VPS Connection

Connect to your VPS using `ssh`. In the line below, replace `your_server_ip` with your VPS IP address
```sh
ssh root@your_server_ip
```

## VPS Apache Removal Node Installation

Remove Apache2 and install Node on your VPS using following lines:
```sh
sudo apt-get purge apache2*
curl -sL https://deb.nodesource.com/setup_20.x -o nodesource_setup.sh
sudo bash nodesource_setup.sh
sudo apt update && sudo apt upgrade -y
```
By the way, we can install node version manager:
```sh
apt install npm
npm install -g n
n latest
```

## Nginx Configuration File

Create a new Nginx configuration file for your Nextjs installation
```sh
sudo nano /etc/nginx/sites-available/nextjs.conf
```
then past the following configuration, replacing `your_server_ip` with your domain name or VPS IP address:
```
server {
    listen 80;
    server_name your_server_ip;
    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Create a symbolic link to enable the configuration:
```
sudo ln -s /etc/nginx/sites-available/nextjs.conf
/etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default.conf
```
Then restart Nginx:
```
sudo service nginx restart
```

## Nextjs application creation

Create a new Nextjs application:
```sh
mkdir /var/www/nextjs
cd /var/www/nextjs
```
Then initialize a new Nextjs app:
```
npx create-next-app@latest mynewapp
```
Once your code source is ready, you can run the dev environment of the app:
```
npm run dev
```
Your Next.js application is now deployed and accessible at your domain name or VPS IP
address. To keep your application running in the background and automatically restart on
crashes or server reboots, you should use a process manager like PM2

If you want to build your app in production mode:
```
npm build
npm start
```

## PM2 Installation

To install PM" globally on your VPS:
```sh
cd ~
sudo npm install -g pm2
```
Navigate to the Nextjs application directory (if not already there) and start the Nextjs application using PM2:
```sh
cd /var/www/nextjs
pm2 start npm --name "nextjs" -- start
```

To ensure PM2 starts on boot, run:
```sh
pm2 startup
```

and save the current process:
```sh
pm2 save
```

## Tutorial Link

Tutorial full explanation can be found [here](/pdf/How_to_Deploy_a_Nextjs_Application_to_a_VPS.pdf)






