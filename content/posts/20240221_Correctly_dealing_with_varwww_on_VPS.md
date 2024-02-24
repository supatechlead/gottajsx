--- 
draft : false
date : 2024-02-21T14:37:18+01:00
title : "Correctly Dealing with /var/www/ Directory on VPS"
tags : ["VPS"]
showTableOfContents : true
type: "post"
---

## Create /var/www/ with Correct Rights

To create the `/var/www` directory with the correct rights, you can do as following:

First connect to your VPS:
```sh
ssh user@your_vps_ip
``` 

Then create the needed directory as a sudoer:
```sh
sudo mkdir /var/www/my_app_react
```

Now set the correct ownership for the created directory:
```sg
sudo chown -R my_user:my_group /var/www/my_app_react
```

Lastly we use `chmod` to set correct and suitable permissions:
```
sudo chmod -R 755 /var/www/my_app_react
```

## Retrieve User Id and Group

To retrieve user `id` ans `group`, do as follwing:
```sh
id -gn user_name
```

```sh
groups user_name
```

## Set Multiple Groups to a Directory

To set multiple groups to a directory:
1. first set a first owner and grup:
```sh
sudo chown nouvel_utilisateur:nouveau_groupe fichier_ou_répertoire
```

2. The, you can use `chgrp` command to add new groups:
```sh
sudo chgrp -R groupe_supplémentaire1,groupe_supplémentaire2 fichier_ou_répertoire
```