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

-	The following command assumes /dev/mmcblk0 is the microSD device
-	The microSD device needs to be equal or larger than 8GB
-	Run as root

```
# cd rev01
# cat $(ls ISSD_UBUNTU_XENIAL_REV01.img.xz.* | sort -V | tr '\n' ' ') | \
	xz -d | \
	pv > /dev/mmcblk0
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
