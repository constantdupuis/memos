# Docker Memo

- [Commande line](#commande-line)

  - [get help](#get-help)

  - [Manage images](#manage-images)

    - [List](#list-images)

    - [Get](#get-image)

    - [remove](#remove-images)

  - [Manage containers](#manage-containers)

    - [Run from Image](#run-from-image)

    - [List container](#list-container)

    - [Remove container](#remove-container)

    - [Start container](#start-container)

    - [Stop Container](#stop-container)

    - [Inspect container](#inspect-container)

  - [Shell-in & container](#shell-in-a-container)

    - [At startup](#at-startup)

    - [On already running container](#on-already-running-container)

- [Networking](#networking)

## Image vs. container

- Image: the application we want to run

- Container: instance of an image running as a process

## Commande line

### Get help

```docker
docker
```

```powershell
docker COMMAND --help
```

### Manage images

#### List images

```powershell
docker images
```

#### Get image

Pull image or repository from registry

```powersehll
docker pull <options> <image_name[TAG|@DIGETS]>
```

#### Remove image

```powershell
docker rmi <image_name/image_id>
```

### Manage containers

#### Run from image

```powersgell
docker run [-it] <image> <commande>
```

- **image** : docker image to use to create the container

- **commande** : commande to start once container is running, with -it it could be bash

- **-it**: start a interactive session

This command set initial parameters for a new container (networks...). All these parameters will be re-used when container is re-started later.

#### List containers

```powershell
docker ps [-a]
```

- -a : to also list not running containers

#### Remove container

```powershell
docker rm <container_id/name> [<container_id/name>...]
```

- container_id : id of the container to remove, taken from list of container

#### Start container

```powershell
docker start <container_id/name>
```

#### Stop container

```powsershell
docker stop <container_id/name>
```

#### Inspect container

Get inner information about the container, network, started commands, config, mounted volumes…

```powershell
docker inspect <container_id/name>
```

# Shell-in a container

## At startup

When starting a new container

```powerhsell
docker run -it <image>  <commande>
```

Provide a command if the image doesn’t provide a default shell at start-up

When starting a existing container

```powerhsell
docker start -ai <container_id>
```

## On already running container

```powershell
docker exec -it <container_id/name> <command>
```

# Networking

- Each container connect to a private virtual network "bridge"

- Each virtual network routes through NAT firewall on host IP

- All containers on a virtual network can talk to each other (without -p)

- Best practice is to create a new virtual network for each app

  - network "my_wab_app" for mysql and php/apache containers

  - network "my_api" for mongo and nodejs containers

- But we can

  - make new virtual networks

  - Attach containers to more then one virtual network (or none)

  - Skip virtual network and use host IP (--net=host)

- Docker **default the hostname to the container's name** (but not with default bridge network), but we can also aliases

## Port mapping

Port mapping between host and container

```powershell
docker run -p [host_ip:]host_port:[cont_ip:]cont_port <image_name>
docker run -p 80:80
docker run -p 80:8080
```

### List ports

```powershell
docker port <container_name/id>
```

### Get container netwok infos

```powershell
docker inspect
```

Get container IP address, look at previous command results to find information key.

```powershell
docker inspect --format "{{ .NetworkSettings.IPAddress }}"
```

### Manage networks

Create network

```powershell
docker network create [OPTIONS] NETWORK_NAME
```

List networks

```powershell
docker network ls
```

Get network informations

```powershell
docker network inspect <network_name>
```

For more help (`connect`, `disconnect` network to containers) see `docker network`

### DNS / How contrainer find each other

Docker **default the hostname to the container's name** if you use a newly ceate bridge network.
