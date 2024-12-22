---
layout: post
title: "TI Board Guide"
date: 2024-12-20 12:00:00 +0800
comments: true
categories: ti am62x am62ax
---

# <center> TI Board Usage Guide

##      1.SK-AM62A-LP usage guide

###     1.1 Build sdk with yocto

####    1.1.1 Prerequisites(one-time setup)
For ubuntu 22.04, please run the following command:
```
$ sudo apt-get update
# Install packages required for builds
$ sudo apt-get -f -y install \
  git build-essential diffstat texinfo gawk chrpath socat doxygen \
  dos2unix python3 bison flex libssl-dev u-boot-tools mono-devel \
  mono-complete curl python3-distutils repo pseudo python3-sphinx \
  g++-multilib libc6-dev-i386 jq git-lfs pigz zstd liblz4-tool \
  cpio file lz4 debianutils iputils-ping python3-git python3-jinja2 \
  python3-subunit locales libacl1 unzip gcc python3-pip python3-pexpect \
  xz-utils wget \
$ sudo locale-gen en_US.UTF-8
```

By default Ubuntu uses “dash” as the default shell for /bin/sh. You must reconfigure to use bash by running the following command:
```
$ sudo dpkg-reconfigure dash

Be sure to select “No” when you are asked to use dash as the default system shell.
```

####    1.1.2 Setting proxy if need
Assume your proxy as below:
```
http://127.0.0.1:7890
```

* git proxy:
```
git config --global http.proxy 'http://127.0.0.1:7890'
git config --global https.proxy 'http://127.0.0.1:7890'
```

* http/https proxy
```
export http_proxy="http://127.0.0.1:7890"
export https_proxy="http://127.0.0.1:7890"
```

* wget proxy
```
$ cat ~/.wgetrc
https_proxy=http://127.0.0.1:7890
http_proxy=http://127.0.0.1:7890
ftp_proxy=http://127.0.0.1:7890
no_proxy = 127.0.0.1
use_proxy=on
```
* apt proxy
```
$ sudo apt-get -o Acquire::http::proxy="http://127.0.0.1:7890" update
$ sudo apt-get -o Acquire::http::proxy="http://127.0.0.1:7890" install xxx
```

####    1.1.3 Download and build steps
```
$ git clone https://git.ti.com/git/arago-project/oe-layersetup.git tisdk
$ cd tisdk
$ ./oe-layertool-setup.sh -f configs/processor-sdk-analytics/processor-sdk-analytics-10.00.00-config.txt
$ cd build
$ . conf/setenv
$ echo 'ARAGO_BRAND = "edgeai"' >> conf/local.conf

//download package only
$ MACHINE=am62axx-evm bitbake -k tisdk-edgeai-image --runall=fetch 

//build image
$ MACHINE=am62axx-evm bitbake -k tisdk-edgeai-image
```

###     1.2 Build sdk with yocto inside Docker(container)

####    1.2.1 install docker
```
$ sudo apt-get  update

sudo apt-get  install ca-certificates curl

sudo install -m 0755 -d /etc/apt/keyrings

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

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

####    1.2.2 install ti docker image
```
$ docker pull ghcr.io/texasinstruments/ubuntu-distro:latest
```

####    1.2.3 Pull the Docker Image & Start a Container
```
host# docker run  --net=host --privileged -it -v ~/projects_docker:/home/tisdk -v /dev:/dev -v /media/:/media/ -w /home/tisdk ghcr.io/texasinstruments/ubuntu-distro:latest
```
or with proxy
```
//wget only support http/https proxy, this cmd works
host# docker run  --net=host --env HTTP_PROXY="http://127.0.0.1:7890" --env http_proxy="http://127.0.0.1:7890" --env HTTPS_PROXY="http://127.0.0.1:7890" --env https_proxy="http://127.0.0.1:7890" --privileged -it -v ~/projects_docker:/home/tisdk -v /dev:/dev -v /media/:/media/ -w /home/tisdk ghcr.io/texasinstruments/ubuntu-distro:latest
```

#### 1.2.4 Download and build yocto steps inside docker

```
tisdk@vmuser-virtual-machine:~$ pwd
/home/tisdk

$ sudo chown -R tisdk /home/tisdk
$ git clone https://git.ti.com/git/arago-project/oe-layersetup.git tisdk
$ cd tisdk
$ ./oe-layertool-setup.sh -f configs/processor-sdk-analytics/processor-sdk-analytics-10.00.00-config.txt
$ cd build
$ . conf/setenv
$ echo 'ARAGO_BRAND = "edgeai"' >> conf/local.conf

//download package only
$ MACHINE=am62axx-evm bitbake -k tisdk-edgeai-image --runall=fetch 

//build image
$ MACHINE=am62axx-evm bitbake -k tisdk-edgeai-image

//build Devkit
$ MACHINE=am62axx-evm bitbake -k meta-toolchain-arago-tisdk
```


## 99. Reference Link
* 1> [SK-AM62A-LP](https://www.ti.com/tool/SK-AM62A-LP)
* 2> [PROCESSOR-SDK-AM62A](https://www.ti.com/tool/PROCESSOR-SDK-AM62A)
* 3> [Processor SDK Linux Software Developer’s Guide](https://software-dl.ti.com/processor-sdk-linux/esd/AM62AX/latest/exports/docs/devices/AM62AX/index.html)
* 4> 