---
layout: post
title: How to avoid conflicts between the Docker Compose virtual network and the host network?
date: 2023-02-22
---

## Networking in Compose

By default Docker Compose sets up a single network for your app. Each container for a service joins the default network and is both reachable by other containers on that network, and discoverable by them at a hostname identical to the container name.

For example, suppose your app is in a directory called `myapp`, and your `docker-compose.yml` looks like this:

```yaml
services:
  web:
    build: .
    ports:
      - "8000:8000"
  db:
    image: postgres
    ports:
      - "8001:5432"
```

When you run `docker compose up`, the following happens:

1. A network called `myapp_default` is created.
2. A container is created using `web`’s configuration. It joins the network `myapp_default` under the name `web`.
3. A container is created using `db`’s configuration. It joins the network `myapp_default` under the name `db`.

The network uses the subnet 198.18.0.0/16, which is reserved for performance testing according to RFC 3330 (fully documented in RFC 2544) and is considered safe for use in a virtual network. It is quite possible that the automatically created network is conflicing company or home's private network.

To avoid conflicts between the Docker virtual network and the host network, we could specify the subnet IP range through:

1. Use pre-exisitng external Docker netowrk.
2. Configure the default network.
3. Change Docker daemon configuration.

### Use Pre-exisitng External Docker Network

The default bridge network created upon Docker installation is docker0. However, this network doesn't support name resolution, so we need to create a new bridge network. Here's the command to create a bridge network with the name docker1 and the specified subnet and gateway:

```sh
$ docker network create \
    --driver=bridge \
    --subnet=198.18.0.0/16 \
    --gateway=198.18.0.1 \
    -o com.docker.network.bridge.name=docker1 \
    docker1
```

In `docker-compose.yml`, we can configure the service to use the docker1 network like this:

```yaml
services:
  xxx:
    image: xxx/xxx:latest
    networks:
      - docker1

networks:
  docker1:
    external: true
```

### Configure the default network/Per-File Docker Network

If you prefer to use default networks, you can specify the subnet like this:

```yaml
networks:
  default:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 198.18.0.0/16
```

### Changing Docker Daemon Setup

To change the Docker daemon setup, edit the file `/etc/docker/daemon.json` and add the following JSON configuration:

```json
{
  "bip": "198.18.251.1/24",
  "default-address-pools": [
    {
      "base": "198.18.252.0/22",
      "size": 26
    }
  ]
}
```

This will set the Docker daemon to use the specified IP address pool for assigning IPs to containers on the Docker network.
