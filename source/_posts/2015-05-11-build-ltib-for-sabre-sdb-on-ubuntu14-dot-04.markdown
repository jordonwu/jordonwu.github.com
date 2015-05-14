---
layout: post
title: "build ltib for sabre sdb on ubuntu14.04"
date: 2015-05-11 14:48:34 +0800
comments: true
categories: 
---
# 1. Package #
    1) L3.0.35_4.1.0_130816_source.gz
	2) 0001_make_L3.0.35_4.1.0_compile_on_Ubuntu_14.04_64bit_OS.patch.zip

# 2. Install LTIB and Apply Patch #
    $ cd ~
	$ tar zxvf L3.0.35_4.1.0_130816_source.tar.gz
	$ ./L3.0.35_4.1.0_130816_source/install 	#suggest install ltib to none root user directory

	$ cd /home/vmuser/ltib_src/ltib
	$ wget https://community.freescale.com/servlet/JiveServlet/download/100725-3-274381/0001_make_L3.0.35_4.1.0_compile_on_Ubuntu_14.04_64bit_OS.patch.zip
	$ unzip 0001_make_L3.0.35_4.1.0_compile_on_Ubuntu_14.04_64bit_OS.patch.zip
	$ git apply 0001_make_L3.0.35_4.1.0_compile_on_Ubuntu_14.04_64bit_OS.patch

# 3. Configure and build LTIB for imx6q-sdb #
	$ cd /home/vmuser/ltib_src/ltib
	
	$ ./ltib -m config
	
	Platform choice (Freescale iMX reference boards)  --->
	
	
	Choose the platform type
	Selection (imx6q)  --->
	
	
	Choose the packages profile
	Selection (use packages in preconfig (Min profile))  --->
	
	
	(imx6q) Platform
	--- LTIB settings
	     System features  --->
	--- Choose the target C library type
	    Target C library type (glibc)  --->
	    C library package (from toolchain only)  --->
	    Toolchain component options  --->
	--- Toolchain selection.
	    Toolchain (ARM, gcc-4.6.2, multilib, neon optimized, gnueabi/eglibc2.13)  --->
	(-O2 -march=armv7-a -mfpu=vfpv3 -mfloat-abi=softfp) Enter any CFLAGS for gcc/g++ (NEW)
	--- Choose your bootloader for U-Boot
	    bootloader (u-boot)  --->
	    u-boot version (u-boot v2009.08)  --->
	--- Choose your board for u-boot
	    board (mx6q_sabresd)  --->
	--- Choose your Kernel
	    kernel (Linux 3.0.35-imx)  --->
	[ ] Always rebuild the kernel
	[ ] Produce cscope index
	--- Include kernel headers
	[*] Configure the kernel
	
	...
	
	--- Package selection 
	    Package list  --->
	--- Target System Configuration
	    Options  --->
	--- Target Image Generation
	    Options  --->
	---
	Load an Alternate Configuration File
	Save Configuration to an Alternate File
	
	
	--- Platform specific package selection
	
	[*] gpu-viv-bin-mx6q
	
	[*] dbus
	
	[*] expat
	
	[*] libjpeg
	
	[*] libpng
	
	
	$ ./ltib
	
	$ config kernel 
	
	Device Driver --->
	MXC support drivers --->
	        MXC Vivante GPU Support --->
	    <*> MXC vivante GPU support

# 4. Create device node #
    $ cd /home/vmuser/ltib_src/ltib
    $ cd rootfs/dev
    $ sudo mknod null c 1 3
    $ sudo chmod 666 null
    
    $ sudo mknod console c 5 1
    $ sudo chmod 777 console
    
    $ for i in 0 1 2 3 4 5 6 7 8 9 10 11 12; do sudo mknod fb$i c 29 $i; done
    $ sudo mknod mxc_ipu c 253 0
    $ sudo mknod mxc_vpu c 252 0
    $ sudo mknod ttymxc0 c 207 16
    $ sudo mknod video0 c 81 0
    $ sudo mknod galcore c 199 0
