---
layout:     post
title:      Converting software raid
---


If you accidentally set up a server with the wrong software raid type, as long as you used LVM you can use mirroring to temporarily move the filesystem off the RAID device, recreate the RAID properly, then move the filesystem back onto it. Needless to say if things go wrong during this process you'll be reinstalling. Below is an example where md1 on sda2 and sdb2 is being converted from RAID0 to RAID1, and an iSCSI volume serves as the temporary storage. In it VG is the volume group name. LV is the only logical volume being moved, and DEVICE is the device linux creates for the iSCSI target. If doing this yourself, you'll want to frequently check the sanity of what you're doing with commands like 'pvs' and 'lvs -a -o +devices'. If feasible, you can save a lot of time by turning off swap, removing its logical volume during the conversion, then recreating it afterwards.

	:::bash
	pvcreate $DEVICE
	vgextend $VG $DEVICE
	lvconvert -m 1 --mirrorlog core ${VG}/$LV $DEVICE
	
	# repeat these two steps for every LV on the VG
	lvconvert -m 0 ${VG}/$LV /dev/md1
	vgreduce $VG /dev/md1
	
	mdadm --stop /dev/md1
	mdadm --remove /dev/md1
	mdadm --create /dev/md1 --level=raid1 --raid-devices=2 /dev/sda2 /dev/sdb2
	mdadm --detail --scan >> /etc/mdadm.conf
	pvcreate /dev/md1
	vgextend $VG /dev/md1
	lvconvert -m 1 --mirrorlog core ${VG}/$LV /dev/md1
	
	# repeat these two steps for every LV on the VG
	lvconvert -m 0 ${VG}/$LV $DEVICE
	vgreduce $VG $DEVICE
	
	mkinitrd -v --with=raid1 /boot/newraid $(uname -r)
	# make the first entry use your new initrd
	vim /boot/grub/grub.conf





