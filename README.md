# docker-multi-php
The goal is to use docker to create a development environment with multiple versions of PHP.

## Milestones
-   Learn how Docker works
-   Learn how best to implement switching between versions and still share configurations for extensions
-   Learn how best switch between versions while using as many layers in common as possible
-   Find a small base image for these purposes to build from

## How Does It All Work?
In this repository, each folder represents an image. Any files in the folder can
be referenced by the respective `Dockerfile`.

## Docker Notes
-   Some recommend running one process per container (httpd, php, etc.)
-   You can name your individual containers (apache, php, etc.); maybe that way,
you can set up apache separate from php and have one image per php version; this
is done in docker-compose.yml
-   When installing OCI8, it looks like you and push the
`instantclient,/usr/local/lib` text to help automate the installation
-   You can also download and add the Oracle libraries to the repository so they
are always available
-   Once you have your images, you can stop one version of PHP and start another
one as needed, hopefully
-   Page that suggested decoupling your docker containers: <https://alysivji.github.io/php-mysql-docker-containers.html>


### Docker CLI Documentation
<https://docs.docker.com/engine/reference/commandline/docker/>

### Commands
#### [Login from the CLI](https://docs.docker.com/engine/reference/commandline/login/)
`docker login`

> Login [using your Docker ID](https://github.com/docker/hub-feedback/issues/935#issuecomment-300361781)
> as your username instead of your email address. You can see it in the upper
> right-hand corner after you log into the [Docker Hub](https://hub.docker.com).

#### [Remove (delete) one or more image](https://docs.docker.com/engine/reference/commandline/image_rm/)
`docker image rm [OPTIONS] IMAGE [IMAGE...]`

> Can specify the image by its name or ID.

#### [See a list of running containers]()
`docker ps`

#### [See all containers, running or not]()
`docker ps --all`

#### [Stop one or more running containers]()
`docker stop CONTAINER [CONTAINER...]`

#### [Stop all running containers](http://blog.baudson.de/blog/stop-and-remove-all-docker-containers-and-images)
`docker stop $(docker ps -aq)`

#### [Stop and remove all running containers]()
`docker stop $(docker ps -aq) && docker rm $(docker ps -aq)`

#### [Delete one or more containers]()
`docker rm CONTAINER [CONTAINER...]`

#### [See a list of images]()
`docker image ls`

#### [Delete all images]
`docker image rm $(docker image ls -aq)`

#### [Stop one or more running containers](https://docs.docker.com/engine/reference/commandline/stop/)
`docker stop [OPTIONS] CONTAINER [CONTAINER...]`

> A container is an image that is running, I suppose, since containers are
> created from images.

#### [Validate a docker-compose.yml file](https://docs.docker.com/compose/reference/config/)
`docker-compose config`

> Run this command in the same folder as the `docker-compose.yml` file.

#### [Start multiple container application detached](https://docs.docker.com/compose/reference/up/)
`docker-compose up -d --build`

#### [Stop and remove containers created via docker-compose up](https://docs.docker.com/compose/reference/down/)
`docker-compose down`

> You also have the option to remove the images if necessary using `--rmi type`.

#### Switch PHP version with a single command
`docker-compose down && docker-compose build --build-arg PHP_VERSION=5.6 && docker-compose up -d`

#### Switch PHP version with a single command & delete intermediate images (good for debugging)
`docker-compose down && docker-compose build --no-cache --force-rm --build-arg PHP_VERSION=5.6 && docker-compose up -d`

#### Open a shell into an Alpine Linux container without Bash
`docker exec -it CONTAINER /bin/sh`

> Alpine Docker images do not have bash installed by default. You can use the
> following command to install it: `RUN apk add --no-cash bash`

### Download

### How It Works

### Images (docker pull)
-   [`alpine` - Alpine Linux](https://hub.docker.com/_/alpine/)
-   [`httpd` - Apache Server (official)](https://hub.docker.com/_/httpd/)
-   [`registry.access.redhat.com/rhel7-rhel-minimal` - RHEL Minimal Base Image](https://access.redhat.com/containers/?tab=images&platform=docker#/registry.access.redhat.com/rhel7-rhel-minimal)
-   [`thomasbisignani/docker-apache-php-oracle` - docker-apache-php-oracle](https://hub.docker.com/r/thomasbisignani/docker-apache-php-oracle/)
