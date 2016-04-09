---
layout:     post
title:      Multipath blocking mdraid
---


I have a system running CentOS 5.5 with 26 drives from Western Digital.


*  2 drives, sda and sdb, are hooked up to an onboard Nivdia MCP55 SATA controller, providing md0 for /boot and md1 for /

*  12 drives, sdo, are hooked up to an ARC-1231 configured as RAID6

*  12 drives, sdc through sdn, are hooked up to an ARC-1231 configured as JBOD

My plan was to do a comparision betweeen hardware and software RAID6. I initialized the 12 disks as a single partition marked as linux raid autodetect. That worked fine.

	:::bash
	for d in /dev/sd{c..n}; do
	  sfdisk -uS $d << EOF
	32,2930276320,fd
	EOF
	done


However, whenever I tried to create the RAID device it failed.

	
	# mdadm --create /dev/md2 --verbose --raid-devices 11 --spare-devices 1 --chunk 128 --level 6 /dev/sd{c..n}1
	mdadm: layout defaults to left-symmetric
	mdadm: Cannot open /dev/sdc1: Device or resource busy
	mdadm: Cannot open /dev/sdd1: Device or resource busy
	mdadm: Cannot open /dev/sde1: Device or resource busy
	mdadm: Cannot open /dev/sdf1: Device or resource busy
	mdadm: Cannot open /dev/sdg1: Device or resource busy
	mdadm: Cannot open /dev/sdh1: Device or resource busy
	mdadm: Cannot open /dev/sdi1: Device or resource busy
	mdadm: Cannot open /dev/sdj1: Device or resource busy
	mdadm: Cannot open /dev/sdk1: Device or resource busy
	mdadm: Cannot open /dev/sdl1: Device or resource busy
	mdadm: Cannot open /dev/sdm1: Device or resource busy
	mdadm: Cannot open /dev/sdn1: Device or resource busy
	mdadm: create aborted


I wasn't able to find any processes that had the devices open, and google didn't provide any advice that seemed relevant. Eventually
I thought to check the device mapper with *dmsetup ls* and saw that the problem was multipathing. After flushing the unused paths
with *multipath -F*, blacklisting the drives in /etc/multipath.conf with the stanza below, and restarting multipathd it worked fine.

	
	device {
	    vendor "WDC"
	}






