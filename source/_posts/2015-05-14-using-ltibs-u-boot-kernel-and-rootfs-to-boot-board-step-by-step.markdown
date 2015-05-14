---
layout: post
title: "using ltib's u-boot kernel and rootfs to boot board step by step"
date: 2015-05-14 14:47:48 +0800
comments: true
categories: ltib imx6
---

# 1. Config and build ltib for sabre sdb imx6q #

# 2. Get source code #
    $ cd path/ltib

    $ ./ltib -m prep -p u-boot
    
    $ cd path/ltib

    $ ./ltib -m prep -p kernel
    
    
# 3. Modify and build u-boot #

    $ cd path/ltib/rpm/BUILD/u-boot-2009.08
    
    $ modify inlcude/configs/mx6q_sabresd.h as below
    
    ////////////////////////////////////////////////////////////////////
    
    #if 0 //ltib default nfs boot
    #define	CONFIG_EXTRA_ENV_SETTINGS					\
    		"netdev=eth0\0"						\
    		"ethprime=FEC0\0"					\
    		"uboot=u-boot.bin\0"			\
    		"kernel=uImage\0"				\
    		"nfsroot=/opt/eldk/arm\0"				\
    		"bootargs_base=setenv bootargs console=ttymxc0,115200\0"\
    		"bootargs_nfs=setenv bootargs ${bootargs} root=/dev/nfs "\
    			"ip=dhcp nfsroot=${serverip}:${nfsroot},v3,tcp\0"\
    		"bootcmd_net=run bootargs_base bootargs_nfs; "		\
    			"tftpboot ${loadaddr} ${kernel}; bootm\0"	\
    		"bootargs_mmc=setenv bootargs ${bootargs} ip=dhcp "     \
    			"root=/dev/mmcblk0p1 rootwait\0"                \
    		"bootcmd_mmc=run bootargs_base bootargs_mmc; "   \
    		"mmc dev 3; "	\
    		"mmc read ${loadaddr} 0x800 0x2000; bootm\0"	\
    		"bootcmd=run bootcmd_net\0"                             \
    
    #else // for mmc boot
    #define	CONFIG_EXTRA_ENV_SETTINGS					\
    		"netdev=eth0\0"						\
    		"ethprime=FEC0\0"					\
    		"uboot=u-boot.bin\0"			\
    		"kernel=uImage\0"				\
    		"nfsroot=/opt/eldk/arm\0"				\
    		"bootargs_base=setenv bootargs console=ttymxc0,115200 \0"\
    		"bootargs_nfs=setenv bootargs ${bootargs} root=/dev/nfs "\
    			"ip=dhcp nfsroot=${serverip}:${nfsroot},v3,tcp\0"\
    		"bootcmd_net=run bootargs_base bootargs_nfs; "		\
    			"tftpboot ${loadaddr} ${kernel}; bootm\0"	\
    		"bootargs_mmc=setenv bootargs ${bootargs} "     \
    			"root=/dev/mmcblk0p1 rootfstype=ext4 rootwait rw " \
    			" ldb=spl0 video=mxcfb0:dev=ldb,LDB-1080P60,if=RGB24,bpp=32 video=mxcfb1:off\0" \
    		"bootcmd_mmc=run bootargs_base bootargs_mmc; " \
    		"mmc dev 3; " \
    		"mmc read ${loadaddr} 0x800 0x2000; bootm\0" \
    		"bootcmd=run bootcmd_mmc\0" \
    
    #endif

    /////////////////////////////////////////////////////////////////////////
    
    $ export ARCH=arm
    $ export CROSS_COMPILE=/opt/gcc-linaro-arm-linux-gnueabi-2012.04-20120426_linux/bin/arm-linux-gnueabi-
    
    $ make mx6q_sabresd_config
    
    $ make
    
    
# 4. Modify and build kernel #
    
    $ cd path/ltib/rpm/BUILD/linux-3.0.35
    
    $ export ARCH=arm
    
    $ export CROSS_COMPILE=/opt/gcc-linaro-arm-linux-gnueabi-2012.04-20120426_linux/bin/arm-linux-gnueabi-
    
    $ make imx6_defconfig
    
    $ make uImage
    
# 5. Reduce rootfs #
    
    $ cd path/ltib/rootfs
    
    $ add post_build.sh
    ##########################################
    #!/bin/sh
    cd dev
    sudo mknod null c 1 3
    sudo chmod 666 null
    
    sudo mknod console c 5 1
    sudo chmod 777 console
    
    for i in 0 1 2 3 4 5 6 7 8 9 10 11 12; do sudo mknod fb$i c 29 $i; done
    
    sudo mknod mxc_ipu c 253 0
    sudo mknod mxc_vpu c 252 0
    sudo mknod ttymxc0 c 207 16
    sudo mknod video0 c 81 0
    sudo mknod galcore c 199 0
    
    cd ..
    sudo rm -rf unit_tests
    sudo rm -rf boot
    sudo rm -rf opt/fsl_samples
    sudo rm -rf opt/viv_samples
    sudo rm -rf share
    
    sudo rm -rf usr/include
    sudo rm -rf usr/info
    sudo rm -rf usr/man
    sudo rm -rf usr/src
    sudo rm -rf usr/share
    
    sudo rm -rf var/spool
    sudo rm -rf var/ftp
    sudo rm -rf var/log
    sudo rm -rf var/run
    sudo rm -rf var/mail
    sudo rm -rf var/lock
    sudo rm -rf var/lib
    sudo rm -rf var/www
    ###############################################
    
    $ ./post_build.sh
    
    $ sudo tar -cjvf ../rootfs.tar.bz2 ./*
    
# 6. Flash u-boot.bin, uImage and rootfs.tar.bz2 to board mmc #

# 7. Reset board. Done! #
