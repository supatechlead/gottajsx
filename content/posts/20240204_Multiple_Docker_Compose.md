--- 
draft : false
date : 2024-01-23T14:37:18+01:00
title : "How to Link Multiple Docker Compose Files"
description : "How to Handle Several Docker Compose Files"
tags : ["Docker"]
showTableOfContents : true
type: "post"
---

The following explain how you can link together severam compose files in just one action by extending multiple `docker-compose` files using `.env` technique or calling multiple configuration files with `docker-compose` command

## Multiple File Call

Suppose we want to manage two differents services, each owning its particular `docker-compose` file

> common.yaml
```yaml
# common.yml
version: '3'
services:
  db:
    image: postgres:11-alpine
    restart: always
    environment:
      POSTGRES_PASSWORD: example
      POSTGRES_USER: example
      POSTGRES_DB: example
```

> app.yaml
```yaml
# app.yml
version: '3'
services:
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db
```

Using `-f` option you will able to call each service files and its dependent services easily.
```bash
docker-compose -f common.yml -f app.yml up
```

## Extending solution
While the extends option in Docker Compose is a convenient way to include services from other files, it is not recommended by Docker. This is because the extends option can introduce complexity and make it more difficult to manage your Docker Compose configuration. 

First, define a `common.yml` file:
```yaml
# common.yml

version: '3'
services:
   nginx_a:
      image: nginx:latest
      container_name: nginx
      ports:
        - "81:80"
        - "1443:443"
```

Next, create a `docker-compose.yml` file that includes the `common.yml` file and its own service as well:
```yaml
# docker-compose.yml

services:
   nginx:
      extends:
         file: common.yml
         service: nginx_a

   nginx_b:
      image: nginx:latest
      container_name: nginx_b
      volumes:
      ports:
        - 82:80
        - 2443:443
```
This will give you two running services: `nginx` and `nginx_b`.

## .env Technique

There is also a technique with `.env` files that allows you to load all the services in the `docker-compose` file without even mentioning them.

To use this technique, create a `.env` file with the following contents:

```
# .env

COMPOSE_PATH_SEPARATOR=:
COMPOSE_FILE=common.yml:docker-compose.yml
```

Then, add another service to the `common.yml` file:
```yaml
# common.yml

version: '3'
services:
  nginx_a:
    image: nginx:latest
    container_name: nginx_a
    ports:
      - 81:80
      - 1443:443

  redis_c:
    image: redis:latest
    container_name: redis_c
    ports:
      - "6381:6380"
```

Finally, load all the services in the `docker-compose.yml` file using the `.env` file:
```yaml
# docker-compose.yml

version: '3'
services:
   nginx_b:
     image: nginx:latest
     container_name: nginx_b
     ports:
       - "82:80"
       - "2443:443"
     env_file:
       - .env
```
Now you will have three running services: `nginx`, `redis_c`, and `nginx_b`.
