---
title: Deploying WordPress on Docker
---

# Overview

# Using the Wordpress Docker Image

```
docker pull wordpress
```

```
docker run -d -p 80:80 -e WORDPRESS_DB_HOST=localhost,WORDPRESS_DB_USER=user1,WORDPRESS_DB_PASSWORD=password,WORDPRESS_DB_NAME=wpsite1 wordpress
```

```
WORDPRESS_DB_HOST=...
WORDPRESS_DB_USER=...
WORDPRESS_DB_PASSWORD=
WORDPRESS_DB_NAME=
WORDPRESS_TABLE_PREF
```

```
docker run -d -p 80:80 --env-file wp_vars wordpress
```



# Customizing the Wordpress Image

```
FROM wordpress

ENV WORDPRESS_DB_HOST=192.168.1.2:3307 \
    WORDPRESS_DB_USER=demo_blog \
    WORDPRESS_DB_PASSWORD=d7FfHZsBASsqN5Y \
    WORDPRESS_DB_NAME=demo_blog \
    WORDPRESS_TABLE_PREFIX=wp_

ADD plugins/plugins-a-1.0.0.tar.gz /var/www/html/wp-content/plugins
ADD plugins/plugin-b-1.1.1.tar.gz /var/www/html/wp-content/plugins
```

```
docker build -t myblog/wordpress:5.2 .
```



# Persistent Storage for Content

``` 
docker run -d -p 80:80 -v /var/www/html/wp-content/themes,/var/www/html/wp-content/plugins,/var/www/html/wp-content/uploads myblog/wordpress:5.2
``` 

```
docker run -d -p 80:80 \
-v themes/:/var/www/html/wp-content/themes \
-v plugins:/var/www/html/wp-content/plugins \
-v uploads/:/var/www/html/wp-content/uploads \
myblog/wordpress:5.2
```

```
docker ps
```

```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
c68d32e31fd2        docker_blog         "docker-entrypoint.s…"   10 hours ago        Up 10 hours         0.0.0.0:80->80/tcp   docker_blog_1
```

```
docker stop c68d32
```

```
docker run -d -p 80:80 \
-v themes/:/var/www/html/wp-content/themes \
-v plugins:/var/www/html/wp-content/plugins \
-v uploads/:/var/www/html/wp-content/uploads \
myblog/wordpress:5.2
```



# Running the Container in Production

## Docker Compose

``` yaml
---
version: '3'
services:
  blog:
    build: .
    ports:
      - "80:80"
    volumes:
      - "./wp-content/:/var/www/html/wp-content:Z"
```

```
docker-compose up -d
```

```
docker-compose ps
```

```
    Name                   Command               State         Ports       
---------------------------------------------------------------------------
docker_blog_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:80->80/tcp
```

``` 
docker-compose stop
```

``` 
docker-compose build
```

```
docker-compose exec blog id
```

```
uid=0(root) gid=0(root) groups=0(root)
```

``` Dockerfile
FROM wordpress
USER www-data

ENV WORDPRESS_DB_HOST=192.168.1.2:3307 \
    WORDPRESS_DB_USER=demo_blog \
    WORDPRESS_DB_PASSWORD=d7FfHZsBASsqN5Y \
    WORDPRESS_DB_NAME=demo_blog \
    WORDPRESS_TABLE_PREFIX=wp_
```

``` yaml
---
version: '3'
services:
  blog:
    build: .
    sysctls:
      - net.ipv4.ip_unprivileged_port_start=0
    ports:
      - "80:80"
    volumes:
      - "./wp-content/:/var/www/html/wp-content:Z"
```



# Container Orchestration


-v themes:/var/www/html/wp-content/themes
-v plugins:/var/www/html/wp-content/plugins
-v uploads:/var/www/html/wp-content/uploads
