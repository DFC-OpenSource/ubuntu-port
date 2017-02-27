Ubuntu port for DFC
===================

-	ID : root  
-	PW : issd

Clone
-----

-	Use the following command to save bandwidth for cloning

```
$ git clone --depth 1 --single-branch -b rev02 https://github.com/DFC-OpenSource/ubuntu-port
```

Validation
----------

-	MD5 : 4a8282f4232705f9328aa5570bb6542e
-	Check the MD5 with the following command :

```
$ cd rev02
$ cat $(ls ISSD_UBUNTU_XENIAL_REV02.img.xz.* | sort -V | tr '\n' ' ') | \
	xz -d | \
	pv -s 7948206080 | \
	md5sum -
```

Installation
------------

-	The following command assumes /dev/mmcblk0 to be the microSD device
-	The microSD device needs to be equal or larger than 8GB
-	Run as root

```
# cd rev02
# cat $(ls ISSD_UBUNTU_XENIAL_REV02.img.xz.* | sort -V | tr '\n' ' ') | \
	xz -d | pv -s 7948206080 | \
	dd of=/dev/mmcblk0 bs=1M oflag=sync
```

Usage
-----

-	Boot the installed Ubuntu by using the following commands from uboot console

```
=> fsl_mc start mc 0x580120010 0x580100010
=> fsl_mc apply dpl 0x5800E0000
=> fatload mmc 0 0xa0000000 uimage
=> fatload mmc 0 0x80000000 device-tree_03.00.00.dtb
=> setenv bootargs "console=ttyS0,115200 root=/dev/mmcblk0p2 rootwait earlycon=uart8250,mmio,0x21c0500,115200 default_hugepagesz=2m hugepagesz=2m hugepages=16 coherent_pool=8M modprobe.blacklist=nvme bootimg=issd panic=5"
=> bootm 0xa0000000 - 0x80000000
```

-	Or if you prefer a single-lined command...

```
fsl_mc start mc 0x580120010 0x580100010 && fsl_mc apply dpl 0x5800E0000 && fatload mmc 0 0xa0000000 uimage && fatload mmc 0 0x80000000 device-tree_03.00.00.dtb && setenv bootargs "console=ttyS0,115200 root=/dev/mmcblk0p2 rootwait earlycon=uart8250,mmio,0x21c0500,115200 default_hugepagesz=2m hugepagesz=2m hugepages=16 coherent_pool=8M modprobe.blacklist=nvme bootimg=issd panic=5" && bootm 0xa0000000 - 0x80000000
```

Changelog
---------

**rev02**

-	Released on 20170930
-	Updated to Ubuntu 16.04.3 LTS (Xenial Xerus)
-	issd-kernel@c08d62b21ea6af182d18c421f08124f9018b0070
-	Kernel - Updated to v4.1.44
-	Kernel - RTL8152/8153 drivers updated to v2.09.00
-	Kernel - Cryptography drivers updated
-	Kernel - IOMMU/VFIO/SMMU enabled
-	Kernel - fsl_mc probing error fixed
-	bootargs updated to fix fsl_mc probing error
-	40Gbit networking now working
-	linux-firmware installed to fix some external devices

**rev01**

-	Initial release
-	Released on 20170227
-	Based on Ubuntu 16.04.2 LTS (Xenial Xerus)
-	issd-kernel@86bc03fb789ba5c864b99d662ef5ca83aa5f99b9
-	apt-fast command included
-	Simple irq balancer included(/etc/irqbalance.sh)
-	/tmp mounted as tmpfs
-	Kernel dpkg image installation automatically installs uimage (/etc/kernel/postinst.d/symlink-to-root)
