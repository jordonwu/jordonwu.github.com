---
layout: post
title: "Docker Usage Summary"
date: 2025-08-17 23:00:00 +0800
comments: true
categories: docker
---

# <center> Docker Usage Summary

## 1. Install docker
```
$ sudo apt-get  update
$ sudo apt-get  install ca-certificates curl
$ sudo install -m 0755 -d /etc/apt/keyrings
$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
$ sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

If your service is maintained by systemd,set http proxy and https proxy in the service config file,/etc/systemd/system/docker.service.d/proxy.conf:

```
$ cat /etc/systemd/system/docker.service.d/proxy.conf
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:7890"
Environment="HTTPS_PROXY=http://127.0.0.1:7890"
```
Then,restart docker service:

```
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

## 2. Build docker image
### 2.1 Build docker image via Dockfile
1) Prepare Dockerfile
2) Using below example command to build docker image
```
$ docker build -t jordonwu/docker-build:ubuntu-22.04 .

or
# set proxy if need
$ docker build --build-arg HTTP_PROXY="http://127.0.0.1:58591/" \
	--build-arg http_proxy="http://127.0.0.1:58591/" \
	--build-arg HTTPS_PROXY="http://127.0.0.1:58591/" \
	--build-arg https_proxy="http://127.0.0.1:58591/"  \
	--network host  -t jordonwu/docker-build:ubuntu-22.04 .
```

## 3. Docker usage
### 3.1 Run docker without sudo
To work better with docker, without sudo, add your user to docker group.
```
$ sudo usermod -aG docker $USER
```

### 3.2. Verify Docker Engine by running the hello-world image
```
$ docker run hello-world
```

### 3.3 Docker delete container

```
$ docker rm CONTAINER_ID

$ docker ps -a
CONTAINER ID   IMAGE                           COMMAND                  CREATED          STATUS                        PORTS                                                                                            NAMES
d9c972f7774f   1b51020b63f5                    "/bin/sh -c 'set -x …"   47 minutes ago   Exited (100) 46 minutes ago

$ docker rm d9c972f7774f
```

### 3.4 Docker delete image

```
$ docker rmi IMAGE_ID
$ docker images
REPOSITORY                TAG                      IMAGE ID       CREATED              SIZE
<none>                    <none>                   1b51020b63f5   About a minute ago   80.6MB
debian                    bullseye-20231030-slim   8546b63bb8a1   2 weeks ago          80.6MB

$ docker rmi 1b51020b63f5
```

### 3.5 Login docker hub
```
$ docker login -u username

Password:

Login Succeeded
```

### 3.6 Push a new image to a registry
First save the new image by finding the container ID (using docker container ls)
and then committing it to a new image name. Note that only a-z0-9-_. are allowed when naming images:
```
$ docker container ps
CONTAINER ID   IMAGE                                COMMAND            CREATED          STATUS          PORTS     NAMES
f6e9e5ec3281   jordonwu/docker-build:ubuntu-22.04   "/entrypoint.sh"   34 seconds ago   Up 34 seconds             docker_build

//commit a container's file changes or settings into a new image
$ docker container commit f6e9e5ec3281 jordonwu/docker-build:ubuntu-22.04

//push image to docker hub
$ docker push jordonwu/docker-build:ubuntu-22.04
```

## 99. Reference Link

1> [Docker reference documentation](https://docs.docker.com/reference/)
