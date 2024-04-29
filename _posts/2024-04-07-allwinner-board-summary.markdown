---
layout: post
title: "allwinner board summary"
date: 2024-04-07 15:00:00 +0800
comments: true
categories: allwinner v3s
---

# allwinner board summary

## 1. licheepi nano board(base on F1C100s)
### 1.1 uboot
```
//install cross compiler first and add to $PATH env variable

//clone code

$ git clone https://github.com/jordonwu/u-boot.git -b master

//config for licheepi_nano
$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- licheepi_nano_defconfig

//build for licheepi_nano
$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf-

//flash to sd card for licheepi_nano
$ sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/sdX bs=1024 seek=8 status=progress

```

## 2. Planck-PI board(base on F1C200s)
### 2.1 uboot
```
//clone code

$ git clone https://github.com/jordonwu/u-boot.git -b master

//config for licheepi_nano
$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- planck_pi_f1c200s_defconfig

//build for licheepi_nano
$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf-

//flash to sd card for licheepi_nano
$ sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/sdX bs=1024 seek=8 status=progress

```

## 3. LicheePi Zero board (base on v3s)
### 3.1 uboot
```
//install cross compiler first and add to $PATH env variable

//clone code

$ git clone https://github.com/jordonwu/u-boot.git -b master

//config for LicheePi_Zero
$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- LicheePi_Zero_defconfig

//build for LicheePi_Zero
$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf-

//flash to sd card for LicheePi_Zero
$ sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/sdX bs=1024 seek=8 status=progress

```

### 3.2 kernel

### 3.3 buildroot (rootfs)


## 4. jbox board(base on v3s chip)
![JBox Board](/static/img/2024-04-07-allwinner-board-summary/jbox/jbox.png)


## 5. BingPi-M1 board (base on v3s chip)
![BingPi-M1 Board](/static/img/2024-04-07-allwinner-board-summary/bingpi-m1/bingpi-m1.png)



## 99. Reference Link
* 1) [BingPi v3s](https://liefyuan.blog.csdn.net/article/details/127263769?spm=1001.2014.3001.5502)
