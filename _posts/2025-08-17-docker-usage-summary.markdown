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

## 2. Run docker without sudo
To work better with docker, without sudo, add your user to docker group.
```
$ sudo usermod -aG docker $USER
```

## 3. Verify Docker Engine by running the hello-world image
```
$ sudo docker run hello-world
```

## 99. Reference Link


