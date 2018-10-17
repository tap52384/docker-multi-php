# docker-multi-php
The goal is to use docker to create a development environment with multiple versions of PHP.

## Milestones
-   Learn how Docker works
-   Learn how best to implement switching between versions and still share configurations for extensions
-   Learn how best switch between versions while using as many layers in common as possible
-   Find a small base image for these purposes to build from

## Docker Notes
### Commands
#### [Login from the CLI](https://docs.docker.com/engine/reference/commandline/login/)
`docker login`

> Login [using your Docker ID](https://github.com/docker/hub-feedback/issues/935#issuecomment-300361781)
> as your username instead of your email address. You can see it in the upper
> right-hand corner after you log into the [Docker Hub](https://hub.docker.com).

#### [Remove (delete) one or more image](https://docs.docker.com/engine/reference/commandline/image_rm/)
`docker image rm [OPTIONS] IMAGE [IMAGE...]`

> Can specify the image by its name or ID.

#### [Stop one or more running containers](https://docs.docker.com/engine/reference/commandline/stop/)
`docker stop [OPTIONS] CONTAINER [CONTAINER...]`

> A container is an image that is running, I suppose...

### How It Works

### Images (docker pull)
-   [`alpine` - Alpine Linux](https://hub.docker.com/_/alpine/)
-   [`httpd` - Apache Server (official)](https://hub.docker.com/_/httpd/)
-   [`registry.access.redhat.com/rhel7-rhel-minimal` - RHEL Minimal Base Image](https://access.redhat.com/containers/?tab=images&platform=docker#/registry.access.redhat.com/rhel7-rhel-minimal)
-   [`thomasbisignani/docker-apache-php-oracle` - docker-apache-php-oracle](https://hub.docker.com/r/thomasbisignani/docker-apache-php-oracle/)
