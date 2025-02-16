---
layout: post
title: "NXP Board Usage Guide"
date: 2024-04-29 10:00:00 +0800
comments: true
categories: nxp imx8 imx9
---

# <center> NXP Board Usage Guide

## 1. imx8mpevk board
### 1.1 build yocto image
```
$ sudo apt install -y gawk wget git-core diffstat unzip texinfo \
    gcc-multilib build-essential chrpath socat file cpio python3 \
    python3-pip python3-pexpect xz-utils debianutils iputils-ping \
    libsdl1.2-dev xterm tar locales net-tools rsync sudo vim curl zstd \
    liblz4-tool libssl-dev bc lzop libgnutls28-dev efitools git-lfs

$ repo init -u https://github.com/jordonwu/nxp-imx_imx-manifest.git -b imx-linux-scarthgap -m imx-6.6.52-2.2.0_custom.xml
$ repo sync -j`nproc`
$ EULA=1 MACHINE=imx8mpevk DISTRO=fsl-imx-xwayland source ./imx-setup-release.sh -b bld-xwayland

//build minimal image
$ bitbake -k core-image-minimal
//download source only
$ bitbake -k core-image-minimal --runall=fetch

//build core image
$ bitbake -k imx-image-core

//build multimedia and graphics image
$ bitbake -k imx-image-multimedia

//build image with multimedia and machine learning and Qt
$ bitbake -k imx-image-full
```

clean package
```
$ bitbake -ccleansstate <package_name>
$ bitbake <package_name>
```

### 1.2 build buildroot image
```

```


## 99. Reference Link

