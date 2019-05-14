---
title: Deploying WordPress on Docker
---

# Overview
Running WordPress sites within a Docker container has grown in popularity since the inception of Docker. Doing so has many obvious benefits, especially for those developing themes and plugins. And while containerizing a WordPress site is not a stranouse activity, building and running a container in production requires a number of considerations. 

In this tutorial you will be progressively guided through each step for running a container in productions, from building the initial image to running it securely and reliabily in production.

# Getting Started

## Running WordPress Containers

WordPress publishes an official Docker image to Dockerhub. It is a bare bones, light weight container running on Alpine Linux. Pull the image down into your local repository.

```
docker pull wordpress
```

Notice that no release version was specified in our pull command. The default action performed by docker pull when no release tag is specified is to pull down the latest image (or <image-name>:latest>. To pull down a specific version of WordPress add the version tag to the docker pull command.
    
```
docker pull wordpress:5.2
```

Let's run the base image to show the container in action. We will use the -p flag to expose the container on port 80 and use the -d flag to daemonize the container to run in the background.

```
docker run -d -p 80:80 wordpress
```

For a typical WordPress install one of the first actions you take will be to configure your database. The base image accepts a number of environment variables used to configure WordPress. The key variables to know are for configuring the database endpoint. 

- WORDPRESS_DB_HOST
- WORDPRESS_DB_USER
- WORDPRESS_DB_PASSWORD
- WORDPRESS_DB_NAME

Docker provides a flag to set a list of environment variables. The following is an example of running the base WordPress image by setting the database related environment variables.

```
docker run -d -p 80:80 -e WORDPRESS_DB_HOST=localhost,WORDPRESS_DB_USER=user1,WORDPRESS_DB_PASSWORD=password,WORDPRESS_DB_NAME=wpsite1 wordpress
```

Now, that isn't likely something you are going to want to type over and over. Thankfully, Docker has a feature for setting environment variables via a text file. Create a new file called wp_vars and then add the following contents. Ensure you set the values to match your environment.

```
WORDPRESS_DB_HOST=
WORDPRESS_DB_USER=...
WORDPRESS_DB_PASSWORD=
WORDPRESS_DB_NAME=
WORDPRESS_TABLE_PREF=wp_
```

To use the environment variables file when running the WordPress container we use the --env-file flag. Run the base image and point the --env-file to the wp_vars file.

```
docker run -d -p 80:80 --env-file wp_vars wordpress
```

You have successfully used the base image to run WordPress as a Docker container. Must sites do not run a vanilla install of WordPress, so in the next section we will look into customizing an image for a site.


## Customizing the Wordpress Image

Dockerfile is a file used by Docker to template the build of a new Docker image. Most images build on top of existing images to extend their configurations. We will do the same by building a new image for a WordPress blog on top of the official WordPress docker image.

Create workspace on your filesystem to store your WordPress docker project.

```
mkdir -p ~/workspace/wordpress
```

Within the workspace, create a new file named Dockerfile and then add the following contents to the Dockerfile.

```
FROM wordpress

ENV WORDPRESS_DB_HOST=192.168.1.2:3307 \
    WORDPRESS_DB_USER=demo_blog \
    WORDPRESS_DB_NAME=demo_blog \
    WORDPRESS_TABLE_PREFIX=wp_
```

Our Dockerfile has two steps: FROM and ENV. The FROM step instructs Docker to build a new image based on the wordpress image. The ENV step allows us to set the envirnonment variables for WordPress. Notice how the WORDPRESS_DB_PASSWORD variable is missing from our Dockerfile. Storing sensitive information, also known as secrets, within our configuration files is very insecure. Instead, we will set the variable when the container is started.

Let's build our new custom image. We will tag our image with a name and version to maintain a history of changes. Since this is our first build, we will tag the image with myblog:1.0.0. 

```
docker build -t myblog:1.0.0 .
```

After a successful build, our image will be available in our local Docker repository. We can verify this by running the docker images command.

```
docker images
```

Let's run our first image. We will set the WordPress database password using the -e flag. Alternatively, we could have used and environment variables file as we did earlier.

```
docker run -d -p 80:80 -e WORDPRESS_DB_PASSWORD=super-secret-password myblog:1.0.0
```

## Adding Plugins and Themes

While a vanilla instance of WordPress might satisfy some, most customize WordPress via additional plugins and themes. Both can be installed via the admin portal. However, since running containers are ephemeral all changes are lost when the container is stopped. 

There are two solutions available. The solution will be covered in this section, and that is to add the plugins to the build process. The second option, attaching peristent storage to the container, will be covered in a section below.

Docker provides to commands to add contents to an image build - ADD and COPY. While both can add downloads plugins and themes to the WordPress image, the ADD command will automatically extract archives to the target.

In your workspace create directoreis to store themes and plugins, and then download some themes and plugins into the appropriate directories.

```
mkdir -p ~/workspace/wordpress/{themes,plugins}
```

Modify the Dockerfile to add the themes and plugins to the WordPress install within your image. For each item we specify a source, on the local machine, and then a target within the image itself.

```
FROM wordpress

ENV WORDPRESS_DB_HOST=192.168.1.2:3307 \
    WORDPRESS_DB_USER=demo_blog \
    WORDPRESS_DB_PASSWORD=d7FfHZsBASsqN5Y \
    WORDPRESS_DB_NAME=demo_blog \
    WORDPRESS_TABLE_PREFIX=wp_

ADD plugins/plugins-a-1.0.0.tar.gz /var/www/html/wp-content/plugins \
    plugins/plugin-b-1.1.1.tar.gz /var/www/html/wp-content/plugins
```

Save your changes and then run a new build. If desired, increment your image version by one.

```
docker build -t myblog:1.0.1 .
```

Run your updated Docker image and verify that the themes and plugins you installed are available. 

```
docker run -d -p 80:80 -e WORDPRESS_DB_PASSWORD=super-secret-password myblog:1.0.1 
```

Adding plugins and themes to your build is convienent, but it creates a few problems. Plugins, for example, usually have a large volume of releases to address bugs and security issues. You could update the plugin from within the running container, however, since the container is ephemeral they updates will not be perserved when the container is restarted. Since updates can introduce database schema changes, your site will fail to load correctly with the outdated containter.

You could generate a new image build with every plugin update, except that will likely become an adminstrative burden. A better solution is to store your plugins and themes in persistent storage, which is mounted every time a new container is started. We cover this topic in the next section.


## Persistent Storage for Content

Docker is able to mount directories on the host filesystem into the container at runtime. This feature allows data to persist beyond the lifecycle of a container, which is ephemeral (temporary). With the volumes feature we can install our plugins and themes on the local host’s filesystem, and all updates will be preserved. 

In this section will use the directory structure we created earlier to host our plugins and themes. We will then mount the directory into our container.

To mount volumes we must use the Docker run -v flag. In the example below we providing a list of target mount points within the container. A source directory will automatically be created by Docker under the /var/lib directory when we don’t include the source. 

``` 
docker run -d -p 80:80 -v /var/www/html/wp-content/themes,/var/www/html/wp-content/plugins,/var/www/html/wp-content/uploads myblog/wordpress:5.2
``` 

Let’s mount our volumes again, but this time we will use the directories we created earlier as our sources. 

```
docker run -d -p 80:80 \
-v themes/:/var/www/html/wp-content/themes \
-v plugins:/var/www/html/wp-content/plugins \
-v uploads/:/var/www/html/wp-content/uploads \
myblog/wordpress:5.2
```

Log into the admin area of WordPress and verify that the plugins and themes we placed in the source directory are available. Now stop the container and run again. 

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

Once again verify the plugins and themes are available. We have now successfully preserved changes to our container without having to add anything to our Docker build. 
We are now free to update our plugins easily without being forced to rebuild our image. We are also free to install plugins and themes from within the admin portal, providing a more natural way of using WordPress. Alternatively, you may still download them to the source directory, where they will be available to the running container. 

Try to install a new plugin from the Admin portal. You will likely be prompted for FTP server information. The reason WordPress behaved in this manner is because the volumes we mounted are owned by the root user. WordPress, running under the context of the www-data user does not have write access. We’ll look at solving this issue in the next section. 

## Running Container With Non-Privileged User

A large number of images available from Dockerhub, including the official WordPress image, run as root. Not surprising, this is not a secure way to run a container. Containers should only ever run with the least level of privileged to function. Going back to our issue writing to mounted volumes in then last section, our volumes are mounted as root and are, therefore, owned by the same account. 

Docker has provided a USER option for the Dockerfiles, which will allows us to run our container as a non-privileged account. The caveat to this is the user account must also exist on the host file system. If it doesn’t already exist, we will have to create it. 

The base WordPress container runs Apache Web Server, and that means WordPress is running as www-data. Let’s modify our Dockerfiles so that our container will also run as www-data. 

Build a new version of your WordPress image and then restart the container. 

Notice that your container has failed to load, as it wasn’t able to bind Apache to port 80. Only privileged users are allowed to bind to port 80, so will will have to instruct our container to allow binding by non-privileged accounts.

Restart your container and verify that it runs successfully. You will now be able to install and update plugins stored on the mounted volume. 


## Handling Failures

Let’s face it, nothing is impervious to failure. There’s a good chance your container will fail for one reason or another, bringing your site offline. Obviously, this is undesirable since our uses expect nothing short of 100% uptime. Docker has several restart policies that can be applied to a container at runtime. These policies will attempt to restart a container after it has either stopped or failed. 




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

## Least Privilege
A container should only run with the least level of privilege needed. Unfortunately, many images found on Dockerhub, such as the official WordPress images, run under the context of root. We can verify this by using the following command against our docker-compose blog service.

```
docker-compose exec blog id
```

The output will display the user id, group id, and associated groups that container runs as. In our example below, we can see that the WordPress container is running as root.

```
uid=0(root) gid=0(root) groups=0(root)
```

Docker provides a USER option for the Dockerfile that allows us to specify the user or uid we want the container to run as. Whichever user is set here must also exist on the host server. You should map the user to that of the service running within the container, and for the WordPress image, which uses Apache Web Server, that means the www-data account. When creating the user on the host system it is important to map it to the same uid used in the image, which is 33. 

Create the www-data user on the host server, if it doesn't already exist.

```
useradd -r -u 33 -g 33 www-data
```

Update the Dockerfile to add `USER www-data` to instruct Docker to run the container under the www-data user context.


``` Dockerfile
FROM wordpress
USER www-data

ENV WORDPRESS_DB_HOST=192.168.1.2:3307 \
    WORDPRESS_DB_USER=demo_blog \
    WORDPRESS_DB_PASSWORD=d7FfHZsBASsqN5Y \
    WORDPRESS_DB_NAME=demo_blog \
    WORDPRESS_TABLE_PREFIX=wp_
```

If you attempted to restart your container with the latest changes, you will note that it will fail to launch successfully. The reason for this is that we are now running unprivileged, preventing Apache from binding to a system port (80). We can update our docker-compose.yml file to instruct the container to allow unprivileged port binding.

Add the following to your docker-compose.yml file under the service (blog) entry.

```
    sysctls:
      - net.ipv4.ip_unprivileged_port_start=0
```

Your docker-compose.yml file should now look like the example below.


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

Rebuild your Docker image via docker-compose and then restart it.

```
docker-compose build && docker-compose up -d
```

# Safely Handling Secrets

We've address running a Docker container reliably on a production server by utilizing docker-compose. We haven't, however, addressed the elephant in the room, which is securely handling secrets. Unfortunately, with WordPress there isn't an easy solution for handling database credentials securely. Whether we are storing this information in our docker-compose.yml file, storing it within our Dockerfile, or entering it as an env variable when executing docker run, the credentials are easily discoverable.

While docker provides a built-in secrets service for handling sensitive information, it is only available in swarm mode. We have not covered docker orchestration solutions in this tutorial, but to address this area of concern you will want to investigate running the container 

# Container Orchestration


-v themes:/var/www/html/wp-content/themes
-v plugins:/var/www/html/wp-content/plugins
-v uploads:/var/www/html/wp-content/uploads
