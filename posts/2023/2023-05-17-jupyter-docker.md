---
layout: post
title: Building Docker Images for Jupyter Notebook
date: 2023-05-17
---

[Jupyter Docker Stacks](https://jupyter-docker-stacks.readthedocs.io/en/latest/index.html) are a set of ready-to-run Docker images containing Jupyter applications and interactive computing tools. We can use a stack image to do any of the following (and more):

- Start a personal Jupyter Server with the JupyterLab frontend (default)
- Run JupyterLab for a team using JupyterHub
- Start a personal Jupyter Notebook server in a local Docker container
- Write your own project Dockerfile

Sometimes, we would like to build customized docker images. Here are some quick examples for how to build them.

```sh
$ git clone https://github.com/jupyter/docker-stacks.git
$ cd docker-stacks
```

- To build a private version of Docker iamges with Python 3.9

```sh
$ OWNER=aaronchenwei DOCKER_BUILD_ARGS="--build-arg PYTHON_VERSION=3.9"  make build-all
```

- To build 

```sh
$ OWNER=aaronchenwei DOCKER_BUILD_ARGS="--build-arg NB_USER=aaron NB_UID=10000 NB_GID=10000"  make build-all
```
