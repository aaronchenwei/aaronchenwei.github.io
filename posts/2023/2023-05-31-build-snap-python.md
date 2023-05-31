---
layout: post
title: Build SNAP with Python 3.10 on Ubuntu 22.04 LTS
date: 2023-05-31
---

**S**tanford **N**etwork **A**nalysis **P**latform (**SNAP**) is a general purpose network analysis and graph mining library. It is written in C++ and easily scales to massive networks with hundreds of millions of nodes, and billions of edges. It efficiently manipulates large graphs, calculates structural properties, generates regular and random graphs, and supports attributes on nodes and edges. 

*Snap.py* is a Python interface for SNAP. It provides performance benefits of SNAP, combined with flexibility of Python. Most of the SNAP C++ functionality is available via Snap.py in Python.

SNAP 6.0 was release on December 2020. There is on wheel files for python 3.10 or 3.11 in pypi site. This blog describes the steps to build SNAP with Python 3.10 on Ubuntu 22.04 LTS.

<p>
<image src="https://github.com/aaronchenwei/aaronchenwei.github.io/assets/9360415/c7f1f8f7-e7c5-4888-88e9-f78b677cd6ba" width='400'>
</p>

## 1. Launch Ubuntu 22.04 Container

- Here is the docker compose file

```yaml
version: "2"

services:
  ubuntu:
    build:
      context: .
      dockerfile: Dockerfile
    image: aaronchenwei/ubuntu-jammy
    container_name: ubuntu-jammy
    network_mode: host
    tty: true
    stdin_open: true
    environment:
      - TZ=Asia/Shanghai
```

- Here is the Dockerfile

```Dockerfile
FROM docker.io/ubuntu:jammy

USER root
WORKDIR /root

RUN sed -i 's@//.*archive.ubuntu.com@//mirrors.aliyun.com@g' /etc/apt/sources.list \
    && sed -i 's/security.ubuntu.com/mirrors.aliyun.com/g' /etc/apt/sources.list \
    && apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y ca-certificates \
    && sed -i 's/http:/https:/g' /etc/apt/sources.list

RUN apt-get install -y vim wget build-essential git python3 python3-pip
```

- Build and Launch Ubuntu 22.04 container

```sh
$ docker compose build
$ docker compose exec ubuntu bash -l
```

We are using user `root` inside container.

## 2. Git clone source code

```sh
# git clone https://github.com/snap-stanford/snap-python.git
# git clone https://github.com/snap-stanford/snap
```

## 3. Install SWIG

```sh
# apt install swig
```

## 4. Build Wheel

- Enter directory `snap-python`.

```sh
# cd snap-pyton
```

- Modify file ``

After line 49 add include path for python 3.10

```Makefile
IFLAGS3 += -I/usr/local/include/python3.10  -I/usr/include/python3.10
```

- Build wheel file

```sh
# cd swig
# make clean-obj
# time make whldist3
```

After building, we can find wheel file `snap_stanford-6.0.0-cp310-cp310-manylinux1_x86_64.whl` under directory `dist`.

## 4. Test wheel

```sh
$ pip3 install snap_stanford-6.0.0-cp310-cp310-manylinux1_x86_64.whl
```
