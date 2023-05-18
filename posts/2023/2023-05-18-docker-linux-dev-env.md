---
layout: post
title: Using Linux Docker Container as Development Environment 
date: 2023-05-18
---

Using a Linux Docker container as a development environment can be a powerful way to ensure consistency and portability across different machines. Here's a step-by-step guide with example on how to set up and use a Linux Docker container for your development workflow.

## 1. Prerequisite

Docker should be installed with following componenets.

- Docker Engine 
- containerd
- Docker Compose Plugin

On host machine with CentOS Linux, we can install `docker-ce` as below.  

```sh
$ sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 2. Choose a Linux Image

Choose a base image: Select a Linux distribution or base image that suits your development needs. For example, you can use `Ubuntu`, `CentOS`, or `Debian` as your base image.

Let's use project [openmp-mpi-examples](https://github.com/aaronchenwei/openmp-mpi-examples) for example. That project is based on OpenMP and MPI. The programming language is C and C++. 

We choose `Ubuntu 22.04 LTS` as the base Linux Image. Then we use `docker compose` to bring a `Ubuntu 22.04` container.

Here is the file `docker-compose.yaml`.

```yaml
version: "2"

services:
  ubuntu:
    image: ubuntu:jammy
    container_name: ubuntu-jammy
    network_mode: host
    tty: true
    stdin_open: true

```

- `newwork_mode: host` means we use host machine's netowrk.

We can use `docker compose` to bring up a container.

```sh
$ docker compose up -d
```

Then we enter the Linux container and launch a new `bash` shell.

```sh
$ docker compose exec ubuntu bash
```

## 3. Setup Devepment Environment 

As we've already been inside that Ubuntu container as user `root`, we can install necessary tools for development setup.

```bash
# apt update
# apt install build-essential git cmake mpich libmpich-dev
```

## 4. Clone Source Code from GitHub

We then clone source code of project `openmp-mpi-examples` from github.

```bash
# cd ~
# git clone https://github.com/aaronchenwei/openmp-mpi-examples.git
# cd openmp-mpi-examples
```

## 5. Build executables

We can make any code changes to the souce code and then build the code.

```bash
# cd mpi/prime/c/
# mkdir build && cd build
# cmake ..
# make
```

We will find executable with name `prime` built. It is a program to calcuate prime number through multiple processes.

Then we can test the program within the container.

```bash
# cd ..
# ./prime_local.sh
```

## 6. Copy executable out of Container

We use `exit` command inside the container and come back to the host server.

```sh
$ docker cp ubuntu-jammy:/root/openmp-mpi-examples/mpi/prime/c/build/prime ./tmp/prime
```


## 7. Build a Sepereate Docker Image

With the execulte, we then create new Docker image for a seperate envrionment. 

Below is the `Dockerfile`.

```Dockerfile
FROM ubuntu:jammy

RUN \
    apt-get update -y \
    && apt-get install --no-install-recommends -y mpich \
    && apt-get purge -y

COPY prime /prime

ENTRYPOINT ["mpiexec", "-v", "-np", "8", "/prime"]
```

We build the new image with tag `aaronchenwei/mpich-prime`

```sh
$ docker build . -t aaronchenwei/mpich-prime 
```

## 8. Test the new Image

We can run the container.

```sh
$ docker run -d -name mpich-prime aaronchenwei/mpich-prime
```

After then, we can check the logs.

```sh
$ docker logs -t mpich-prime
```
