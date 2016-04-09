---
layout:     post
title:      Online resizing with multipath and lvm
---


Here is a walkthrough on how to add more disk space to a system using multipathing and LVM without needing any downtime. In addition to showing the commands necessary to perfom the capacity expansion it is intended to show how, even if you didn't know how the storage on a server was set up, you could figure it out. It assumes you're using RHEL5 and a filesystem mounted on a device from a SAN that has more space available. In this example, I want to resize */u02* on a system named eserver.

## On the server

First, I must identify the devices that back the filesystem I want to resize.

Looking in */proc/partitions*, I see that */u02* is mounted on */dev/mapper/vg--esan1-lv--oracle*

From that name, it looks like this is a logical volume. Thus, I use `lvdisplay` and see that from LVM's perspective the logical volume is named */dev/vg-esan1/lv-oracle* and is in the volume group *vg-esan1*.

I use `vgdisplay vg-esan1` to see if there is enough space left in the volume group for me to grow the logical volume by the necessary amount without adding more space to the volume group. There is not.

I use `pvdisplay` to see what physical volumes *vg-esan1* is stored on. There is only one, */dev/dm-6*.

Now, what's */dev/dm-6*? I use `dmsetup info /dev/dm-6` to get more information. I see that the name is *eserver-oraclep1*; the *p1* at the end tells me that this is the first partition on another device that has the name *eserver-oracle*. I see that the UUID starts with *part1-mpath*. This gives me even more information, it's the first partition on a multipath device.

Now I need to get more information about the multipath device named *eserver-oracle*. What I'm looking for are the devices that represent the different paths. To list them, I use `multipath -l eserver-oracle`. Sure enough, the device exists, it's */dev/dm-3*, and it's made up of */dev/sdd*, *dev/sdi*, */dev/sdh*, and *dev/sdm*. I see that the device is on a SUN CSM200R, which lets me know that it is on the hardware we call esan. I also see that the WWID is *3600a0b80002ae56a00000b844a031141*; this will let me know which volume to resize on the esan.

## On the SAN

This example is for an oracle storagetek device.

After logging in the the web interface, I look at the volumes for one that matches the WWID I saw from `multipath -l eserver-oracle`. I realize that the first character of the WWID isn't a part of the ID as displayed here, but after I drop that there's a volume with the ID *60:0A:0B:80:00:2A:E5:6A:00:00:0B:84:4A:03:11:41* named *ext3-vol2*.

It's on *virtual disk 5*, so I pull up *virtual disk 5* and click the *Expand* button. it walks me through adding another disk, but in the end it won't let me because the virtual disk is in a storage pool that is associated with a profile that has a fixed volume size of five disks.

I go back to the page for *ext3-vol2* and move it to the pool named *default*, which has the same settings of the pool it was already in except for a fixed number of disks.

Now I go back to the page for *virtual disk 5* and can successfully add a disk. After doing this, the virtual disk status is reported as "dynamic capacity expansion"; this last for several hours.

After the "dynamic capacity expansion" operation completes, I go back to the page for *ext3-vol2*, click the *Expand* button, and am able to expand it to use the new space on its virtual disk.

## Back on the server

Earlier we discovered things from the filesystem down. Now, we have to resize them in the reverse order.

### Resize the paths in the multipath device

	
	echo 1 > /sys/block/sdd/device/rescan
	echo 1 > /sys/block/sdi/device/rescan
	echo 1 > /sys/block/sdh/device/rescan
	echo 1 > /sys/block/sdm/device/rescan


### Resize the multipath device

It's important that there is no space after *-k*.

`multipathd -k'resize map eserver-oracle'`

### Resize the partition on the device

There are two parts to this. Performing the resize, and getting the device mapper to realize that the partition has been resized.

To perform the resize, use `fdisk /dev/dm-3` to delete the existing partition and recreate another of the same type that takes uses all available space.

Getting the resize recognized is tricker. You might think that `partprobe /dev/dm-3` or `blockdev --rereadpt /dev/dm-3` would do it. That might be true for regular block device, but not for the virtual block devices created by device mapper. Instead what you have to do is dump to a file the device mapper configuration for the device that represents the partition, edit the size, suspend the device, reload the configuration from the edited file, and resume the device. While the device is suspended writes to it are blocked, but no data is lost. You can use `kpartx -l /dev/dm-3` to get the new size of the partition. Alternately, if `kpartx` is not installed you can do `fdisk -lu /dev/dm-3`, subtract *start* from *end*, and add one to get the new size. It's important that you use to *-u* flag to fdisk so that *start* and *end* are reported in sectors, not cylinders. Once you know the size, the process looks like

	
	dmsetup table /dev/dm-6 > dm-6-table
	vim dm-6-table
	dmsetup suspend /dev/dm-6
	dmsetup reload /dev/dm-6 dm-6-table
	dmsetup resume /dev/dm-6


### Resize the LVM physical volume

`pvresize /dev/dm-6`

### Resize the LVM logical volume

I used `vgdisplay vg-esan1` to see how much space was available. It's a good idea to keep some in reserve for snapshots or emergencies.

`lvextend -L +240G /dev/vg-esan1/lv-oracle /dev/dm-6`

### Resize the filesystem

`resize2fs -p /dev/mapper/vg--esan1-lv--oracle`

Now you're done!




