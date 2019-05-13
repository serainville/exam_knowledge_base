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

ENV WORDPRESS_DB_HOST=...
ENV WORDPRESS_DB_USER=...
ENV WORDPRESS_DB_PASSWORD=
ENV WORDPRESS_DB_NAME=
ENV WORDPRESS_TABLE_PREF

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
docker stop 123
```

```
docker run -d -p 80:80 \
-v themes/:/var/www/html/wp-content/themes \
-v plugins:/var/www/html/wp-content/plugins \
-v uploads/:/var/www/html/wp-content/uploads \
myblog/wordpress:5.2
```



# Running the Container in Production

# Container Orchestration


-v themes:/var/www/html/wp-content/themes
-v plugins:/var/www/html/wp-content/plugins
-v uploads:/var/www/html/wp-content/uploads
