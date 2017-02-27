Ubuntu port for DFC
===================

-	ID : root  
-	PW : issd

Validation
----------

-	MD5 : 1d91a85610294ff596ff4a2a70517a79  
-	Check the MD5 with the following command :

```
$ cd rev01
$ cat $(ls ISSD_UBUNTU_XENIAL_REV01.img.xz.* | sort -V | tr '\n' ' ') | \
	xz -d | \
	pv | \
	md5sum -
```

Installation
------------

-	The following command assumes /dev/mmcblk0 to be the microSD device
-	The microSD device needs to be equal or larger than 8GB
-	Run as root

```
# cd rev01
# cat $(ls ISSD_UBUNTU_XENIAL_REV01.img.xz.* | sort -V | tr '\n' ' ') | \
	xz -d | \
	pv > /dev/mmcblk0
```

Usage
-----

-	Boot the installed Ubuntu by using the following commands from uboot console

```
=> fatload mmc 0 0xa0000000 uimage
=> fatload mmc 0 0x80000000 device-tree_03.00.00.dtb
=> setenv bootargs "console=ttyS0,115200 root=/dev/mmcblk0p2 rootwait earlycon=uart8250,mmio,0x21c0500,115200 default_hugepagesz=2m hugepagesz=2m hugepages=16 modprobe.blacklist=nvme bootimg=issd panic=5"
=> bootm 0xa0000000 - 0x80000000
```

Changelog
---------

**rev01**

-	Initial release
-	Released on 20170227
-	Based on Ubuntu 16.04.2 LTS (Xenial Xerus)
-	issd-kernel@86bc03fb789ba5c864b99d662ef5ca83aa5f99b9
-	apt-fast command included
-	Simple irq balancer included(/etc/irqbalance.sh)
-	/tmp mounted as tmpfs
-	Kernel dpkg image installation automatically installs uimage (/etc/kernel/postinst.d/symlink-to-root)
