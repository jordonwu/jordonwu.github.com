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

# 2. Add user super privilege to visudo ##
	
    $sudo /usr/sbin/visudo

	#add below line for vmuser account super privilege
	vmuser ALL = NOPASSWD: /usr/bin/rpm, /opt/freescale/ltib/usr/bin/rpm

# 3. Install LTIB and Apply Patch #
    $ cd ~
	$ tar zxvf L3.0.35_4.1.0_130816_source.tar.gz
	$ ./L3.0.35_4.1.0_130816_source/install 	#suggest install ltib to none root user directory

	$ cd /home/vmuser/ltib_src/ltib
	$ wget https://community.freescale.com/servlet/JiveServlet/download/100725-3-274381/0001_make_L3.0.35_4.1.0_compile_on_Ubuntu_14.04_64bit_OS.patch.zip
	$ unzip 0001_make_L3.0.35_4.1.0_compile_on_Ubuntu_14.04_64bit_OS.patch.zip
	$ git apply 0001_make_L3.0.35_4.1.0_compile_on_Ubuntu_14.04_64bit_OS.patch

# 4. Configure and build LTIB for imx6q-sdb #
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

# 5. Create device node #
## 5.1 Create device node manual ##
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
    $ sudo mknod video0  c 81  5
	$ sudo mknod video1  c 81  6
	$ sudo mknod video16 c 81  0
	$ sudo mknod video17 c 81  1
	$ sudo mknod galcore c 199 0

## 5.2 Create device node via device_table.txt  ##
	$ edit ltib/bin/device_table.txt to add your device node as below

	......
	# Normal system devices
	/dev/mem		c	640	0	0	1	1	0	0	-
	/dev/kmem		c	640	0	0	1	2	0	0	-
	/dev/null		c	666	0	0	1	3	0	0	-
	/dev/zero		c	666	0	0	1	5	0	0	-
	/dev/random		c	666	0	0	1	8	0	0	-
	/dev/urandom	c	666	0	0	1	9	0	0	-
	/dev/ram		b	640	0	0	1	1	0	0	-
	/dev/ram		b	640	0	0	1	0	0	1	4
	/dev/loop		b	640	0	0	7	0	0	1	2
	/dev/rtc		c	640	0	0	10	135	-	-	-
	/dev/console	c	666	0	0	5	1	-	-	-
	/dev/tty		c	666	0	0	5	0	-	-	-
	/dev/tty		c	666	0	0	4	0	0	1	8
	/dev/ttyp		c	666	0	0	3	0	0	1	10
	/dev/ptyp		c   666 0   0   2   0   0   1   10
	/dev/ptmx		c	666	0	0	5	2	-	-	-
	/dev/ttyP		c	666	0	0	57	0	0	1	4
	/dev/ttyS		c	666	0	0	4	64	0	1	4
	/dev/fb			c	640	0	5	29	0	0	32	4
	#/dev/ttySA		c	666	0	0	204	5	0	1	3
	/dev/psaux		c	666	0	0	10	1	0	0	-
	/dev/ppp		c	666	0	0	108	0	-	-	-
	/dev/ttyAM		c	666	0	0	204	16	0	1	4
	/dev/ttyCPM		c	666	0	0	204	46	0	1	2
	
	# iMX use /dev/ttymxc0 as default console
	/dev/ttymxc		c	660	0	0	207	16	0	0	1
	......

	$  ./ltib -p dev -f //refresh dev node to rootfs 

# 6. Modify /etc/inittab #
	//copy from buildroot

	# /etc/inittab
	#
	# Copyright (C) 2001 Erik Andersen <andersen@codepoet.org>
	#
	# Note: BusyBox init doesn't support runlevels.  The runlevels field is
	# completely ignored by BusyBox init. If you want runlevels, use
	# sysvinit.
	#
	# Format for each entry: <id>:<runlevels>:<action>:<process>
	#
	# id        == tty to run on, or empty for /dev/console
	# runlevels == ignored
	# action    == one of sysinit, respawn, askfirst, wait, and once
	# process   == program to run
	
	# Startup the system
	null::sysinit:/bin/mount -t proc proc /proc
	null::sysinit:/bin/mount -o remount,rw /
	null::sysinit:/bin/mkdir -p /dev/pts
	null::sysinit:/bin/mkdir -p /dev/shm
	null::sysinit:/bin/mount -a
	null::sysinit:/bin/hostname -F /etc/hostname
	# now run any rc scripts
	::sysinit:/etc/init.d/rcS
	
	# Put a getty on the serial port
	ttymxc0::respawn:/sbin/getty -L  ttymxc0 0 vt100 # GENERIC_SERIAL
	
	# Stuff to do for the 3-finger salute
	::ctrlaltdel:/sbin/reboot
	
	# Stuff to do before rebooting
	::shutdown:/etc/init.d/rcK
	::shutdown:/sbin/swapoff -a
	::shutdown:/bin/umount -a -r

# 7. ltib/rootfs is your rootfs #
