# docker-multi-php
The goal is to use docker to create a development environment with multiple versions of PHP.

## Milestones
-   Learn how Docker works
-   Learn how best to implement switching between versions and still share configurations for extensions
-   Learn how best switch between versions while using as many layers in common as possible
-   Find a small base image for these purposes to build from

## How Does It All Work?
-   One image is used for Apache, another for PHP.

In this repository, each folder represents an image. Any files in the folder can
be referenced by the respective `Dockerfile`.

## DNSMasq
### macOS Installation Instructions
-   Install [Homebrew](https://brew.sh/) if you haven't already
-   Install DNSMasq via brew: `brew install dnsmasq`
-   Configure DNSMasq to point all `*.test` hosts to `localhost`:
```bash
echo 'address=/.test/127.0.0.1' > /usr/local/etc/dnsmasq.conf
```
-   Make sure the dnsmasq service runs automatically as root on boot
```bash
sudo brew services start dnsmasq
```
-   Lastly, add it to the list of resolvers:
```bash
sudo mkdir -v /etc/resolver
sudo bash -c 'echo "nameserver 127.0.0.1" > /etc/resolver/test'
```
-   Now you can ping any *.test domain, like `bogus.test` and it should work:
```
ping bogus.test
PING bogus.test (127.0.0.1): 56 data bytes
64 bytes from 127.0.0.1: icmp_seq=0 ttl=64 time=0.044 ms
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.118 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.045 ms
```

This sets up wildcard forwarding for all `*.test` DNS names to localhost.

## Docker Notes
-   Some recommend running one process per container (httpd, php, etc.)
-   You can name your individual containers (apache, php, etc.); maybe that way,
you can set up apache separate from php and have one image per php version; this
is done in docker-compose.yml
-   When installing OCI8 via PECL, it looks like you and push the
`instantclient,/usr/local/lib` text to help automate the installation with
`printf` or something like that
-   You can also download and add the Oracle libraries to the repository so they
are always available
-   Once you have your images, you can stop one version of PHP and start another
one as needed, hopefully
-   Page that suggested decoupling your containers: <https://alysivji.github.io/php-mysql-docker-containers.html>
-   Official documentation also suggesting [decoupling applications](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#decouple-applications)
-   It's normal to see `[Warning] One or more build-args [PHP_VERSION] were not consumed`
as the PHP_VERSION variable is only used in the `php` Dockerfile


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
`docker-compose down && docker-compose build --no-cache --force-rm --build-arg PHP_VERSION=7.2 && docker-compose up -d`

#### Open a shell into an Alpine Linux container without Bash
`docker exec -it CONTAINER /bin/sh`

> Alpine Docker images do not have bash installed by default. You can use the
> following command to install it: `RUN apk add --no-cash bash`

### Images (docker pull)
-   [`alpine` - Alpine Linux](https://hub.docker.com/_/alpine/)
-   [`httpd` - Apache Server (official)](https://hub.docker.com/_/httpd/)
-   [`registry.access.redhat.com/rhel7-rhel-minimal` - RHEL Minimal Base Image](https://access.redhat.com/containers/?tab=images&platform=docker#/registry.access.redhat.com/rhel7-rhel-minimal)
-   [`thomasbisignani/docker-apache-php-oracle` - docker-apache-php-oracle](https://hub.docker.com/r/thomasbisignani/docker-apache-php-oracle/)



## TODO
-   Install Oracle drivers
-   Fix it so that build-args are consumed so that the host path can be set
upon build
-   Think about how to 
