# Tags
> _Built from [`quay.io/ibmz/alpine:3.12`](https://quay.io/repository/ibmz/alpine?tab=info)_
-	[`1.27.4`](https://github.com/lcarcaramo/compose/blob/1.27.x/Dockerfile) - [![Build Status](https://travis-ci.com/lcarcaramo/compose.svg?branch=master)](https://travis-ci.com/lcarcaramo/compose)

# What is Docker Compose

![Docker Compose](logo.png?raw=true "Docker Compose Logo")

Compose is a tool for defining and running multi-container Docker applications.
With Compose, you use a Compose file to configure your application's services.
Then, using a single command, you create and start all the services
from your configuration. To learn more about all the features of Compose
see [the list of features](https://github.com/docker/docker.github.io/blob/master/compose/index.md#features).

Compose is great for development, testing, and staging environments, as well as
CI workflows. You can learn more about each case in
[Common Use Cases](https://github.com/docker/docker.github.io/blob/master/compose/index.md#common-use-cases).

# How to Use This Image

1. Create your `docker-compose.yml` file and other resources required for the containers specified in `docker-compose.yml`.
```
version: '2'

services:
  java:
    image: quay.io/ibmz/openjdk:11.0.8
    command: java --version
```
2. Create a `Dockerfile` that builds an image that contains your `docker-compose.yml` file, and other resources required for the containers specified in `docker-compose.yml`.
```
FROM quay.io/ibmz/compose:1.27.4

COPY docker-compose.yml /home/

ENV COMPOSE_FILE=/home/docker-compose.yml

CMD ["docker-compose", "up"]
```
3. Build the image.
`docker build -t docker-compose-app .`
4. Run a container using the image you just built. _(Ensure that you are mounting `docker.sock` to the container)_
`docker run --name app -v /var/run/docker.sock:/var/run/docker.sock:ro docker-compose-app`

# Installation and documentation

- Full documentation is available on [Docker's website](https://docs.docker.com/compose/).
- Code repository for Compose is on [GitHub](https://github.com/lcarcaramo/compose).

# License 

Compose is licensed under the __Apache 2.0__ license. See more details __[here](https://github.com/docker/compose/blob/master/LICENSE)__
